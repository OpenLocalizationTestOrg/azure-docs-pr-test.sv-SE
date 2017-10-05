---
title: "Guide för att skapa en datatjänst för Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar för hur du skapar, certifiera och distribuera en Data-tjänst för att köpa på Azure Marketplace."
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
ms.openlocfilehash: a853b4dbd1952ba4ea8ee68ea3ca98f588bb71a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a><span data-ttu-id="9792d-103">Mappning av en befintlig webbtjänst till OData via CSDL</span><span class="sxs-lookup"><span data-stu-id="9792d-103">Mapping an existing web service to OData through CSDL</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9792d-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="9792d-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="9792d-105">Om du har ett SaaS-affärsprogram som du vill publicera på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="9792d-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="9792d-106">Om du har en IaaS-program eller developer-tjänsten som du vill publicera på Azure Marketplace du hittar mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="9792d-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="9792d-107">Den här artikeln ger en översikt på hur du använder en CSDL för att mappa en befintlig tjänst till en kompatibel OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="9792d-107">This article gives an overview on how to use a CSDL to map an existing service to an OData compatible service.</span></span> <span data-ttu-id="9792d-108">Den förklarar hur för att skapa mappning dokumentet (CSDL) som omvandlar en inkommande begäran från klienten via ett och utdata (data) tillbaka till klienten via en kompatibel OData-feed.</span><span class="sxs-lookup"><span data-stu-id="9792d-108">It explains how to create the mapping document (CSDL) that transforms the input request from the client via a service call and the output (data) back to the client via an OData compatible feed.</span></span> <span data-ttu-id="9792d-109">Microsoft Azure Marketplace visar slutanvändarna-tjänster med hjälp av OData-protokollet.</span><span class="sxs-lookup"><span data-stu-id="9792d-109">Microsoft’s Azure Marketplace exposes services to the end-users by using the OData protocol.</span></span> <span data-ttu-id="9792d-110">Tjänster som exponeras av innehållsleverantörer (dataägare) visas i flera olika former REST, SOAP, t.ex.</span><span class="sxs-lookup"><span data-stu-id="9792d-110">Services that are exposed by content providers (Data Owners) are exposed in a variety of forms, such as REST, SOAP, etc.</span></span>

## <a name="what-is-a-csdl-and-its-structure"></a><span data-ttu-id="9792d-111">Vad är en CSDL och dess struktur?</span><span class="sxs-lookup"><span data-stu-id="9792d-111">What is a CSDL and its structure?</span></span>
<span data-ttu-id="9792d-112">En CSDL (konceptuellt Schema Definition Language) är en specifikation som definierar hur att beskriva webbtjänst eller database-tjänsten i vanliga XML ordflöde på Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9792d-112">A CSDL (Conceptual Schema Definition Language) is a specification defining how to describe web service or database service in common XML verbiage to the Azure Marketplace.</span></span>

<span data-ttu-id="9792d-113">Enkel översikt över de **flödet för begäran:**</span><span class="sxs-lookup"><span data-stu-id="9792d-113">Simple overview of the **request flow:**</span></span>

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

<span data-ttu-id="9792d-114">Den **dataflöde** i motsatt riktning:</span><span class="sxs-lookup"><span data-stu-id="9792d-114">The **data flow** is in the opposite direction:</span></span>

  `Client <- Azure Marketplace <- Content Provider’s WebService`

<span data-ttu-id="9792d-115">**Bild 1** diagram en klient skulle hur hämta data från en innehållsleverantör (din tjänst) genom att gå via Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9792d-115">**Figure 1** diagrams how a client would obtain data from a content provider (your service) by going through the Azure Marketplace.</span></span>  <span data-ttu-id="9792d-116">CSDL används av komponenten mappning-transformering för att hantera begäran och data som skickas mellan den innehållsleverantören tjänster och den begärande klienten.</span><span class="sxs-lookup"><span data-stu-id="9792d-116">The CSDL is used by the Mapping/Transformation component to handle the request and data pass between the content provider’s service(s) and the requesting client.</span></span>

<span data-ttu-id="9792d-117">*Bild 1: Detaljerad flödar från begärande klienten till innehållsleverantören via Azure Marketplace*</span><span class="sxs-lookup"><span data-stu-id="9792d-117">*Figure 1: Detailed flow from requesting client to content provider via Azure Marketplace*</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

<span data-ttu-id="9792d-119">För bakgrundsinformation om Atom Atom Pub och OData-protokollet som skapar Azure Marketplace-tillägg, hittar: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span><span class="sxs-lookup"><span data-stu-id="9792d-119">For background on Atom, Atom Pub, and the OData protocol upon which the Azure Marketplace extensions build, please review: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span></span>

<span data-ttu-id="9792d-120">Utdrag ovan länk: *”syftet med den Open Data protocol (kallas nedan OData) är att tillhandahålla ett REST-baserat protokoll för CRUD-format åtgärder (skapa, läsa, uppdatera och ta bort) mot resurser visas som data services. En ”datatjänst” är en slutpunkt har angett namn / värde-par där det finns data exponeras från en eller flera ”samlingar” varje med noll eller flera ”poster”, som består av. OData har publicerats av Microsoft under OASIS (organisation för framflyttning för strukturerade Information standarden) standarder så att vem som helst som vill skapa servrar, klienter eller verktyg utan royalty eller begränsningar ”.*</span><span class="sxs-lookup"><span data-stu-id="9792d-120">Excerpt from above link: *“The purpose of the Open Data protocol (hereafter referred to as OData) is to provide a REST-based protocol for CRUD-style operations (Create, Read, Update and Delete) against resources exposed as data services. A “data service” is an endpoint where there is data exposed from one or more “collections” each with zero or more “entries”, which consist of typed named-value pairs. OData is published by Microsoft under OASIS (Organization for the Advancement of Structured Information Standards) Standards so that anyone that wants to can build servers, clients or tools without royalties or restrictions.”*</span></span>

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a><span data-ttu-id="9792d-121">Tre viktiga uppgifter som ska definieras av CSDL är:</span><span class="sxs-lookup"><span data-stu-id="9792d-121">Three Critical Pieces that have to be defined by the CSDL are:</span></span>
* <span data-ttu-id="9792d-122">Den **endpoint** för Service Provider i Web adress (URI) för tjänsten</span><span class="sxs-lookup"><span data-stu-id="9792d-122">The **endpoint** of the Service Provider The Web Address (URI) of the Service</span></span>
* <span data-ttu-id="9792d-123">Den **dataparametrar** som skickas som indata till tjänstprovidern definitionerna av de parametrar som skickas till den innehållsleverantören service till datatypen.</span><span class="sxs-lookup"><span data-stu-id="9792d-123">The **data parameters** being passed as input to the Service Provider The definitions of the parameters being sent to the Content Provider’s service down to the data type.</span></span>
* <span data-ttu-id="9792d-124">**Schemat** av data som returneras till tjänsten begär schemat för de data som skickas av den innehållsleverantören tjänsten, inklusive behållare, samlingar-tabeller, variabler eller kolumner och datatyper.</span><span class="sxs-lookup"><span data-stu-id="9792d-124">**Schema** of the data being returned to the Requesting Service The schema of the data being delivered by the Content Provider’s service, including Container, collections/tables, variables/columns, and data types.</span></span>

<span data-ttu-id="9792d-125">Följande diagram visar en översikt över flödet från där klienten anger OData-uttryck (anrop till innehåll provider-webbtjänsten) att få tillbaka Resultatdata.</span><span class="sxs-lookup"><span data-stu-id="9792d-125">The following diagram shows an overview of the flow from where the client enters the OData statement (call to the content provider’s web service) to getting the results/data back.</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a><span data-ttu-id="9792d-127">Så här:</span><span class="sxs-lookup"><span data-stu-id="9792d-127">Steps:</span></span>
1. <span data-ttu-id="9792d-128">Klienten skickar begäran via tjänsten anropet slutfördes med indataparametrar som definierats i XML-filen på Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="9792d-128">Client sends request via Service call complete with Input Parameters defined in XML to the Azure Marketplace</span></span>
2. <span data-ttu-id="9792d-129">CSDL används för att verifiera anropet till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9792d-129">CSDL is used to validate the Service call.</span></span>
   * <span data-ttu-id="9792d-130">Tjänsten formaterad anropa sedan skickas till tjänsten innehåll Providers av Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="9792d-130">The Formatted Service Call is then sent to the Content Providers Service by the Azure Marketplace</span></span>
3. <span data-ttu-id="9792d-131">Webbtjänsten som körs och preforms för Http-verbet (d.v.s. får) data returneras till Azure Marketplace där den begärda informationen (eventuella) är visar i XML-Format till klienten med hjälp av mappningen som definierats i CSDL.</span><span class="sxs-lookup"><span data-stu-id="9792d-131">The Webservice executes and preforms the action of the Http Verb (i.e. GET) The data is returned to Azure Marketplace where the requested data (if any) is exposes in XML Format to the Client using the Mapping defined in the CSDL.</span></span>
4. <span data-ttu-id="9792d-132">Klienten skickas data (om sådan finns) i XML- eller JSON-format</span><span class="sxs-lookup"><span data-stu-id="9792d-132">The Client is sent the data (if any) in XML or JSON format</span></span>

## <a name="definitions"></a><span data-ttu-id="9792d-133">Definitioner</span><span class="sxs-lookup"><span data-stu-id="9792d-133">Definitions</span></span>
### <a name="odata-atom-pub"></a><span data-ttu-id="9792d-134">OData-ATOM pub</span><span class="sxs-lookup"><span data-stu-id="9792d-134">OData ATOM pub</span></span>
<span data-ttu-id="9792d-135">Ange ett tillägg till den ATOM pub där varje post representerar en rad med ett resultat.</span><span class="sxs-lookup"><span data-stu-id="9792d-135">An extension to the ATOM pub where each entry represents one row of a result set.</span></span> <span data-ttu-id="9792d-136">Innehåll tillhör transaktionen är utökat för att innehålla värdena på raden – som nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="9792d-136">The content part of the entry is enhanced to contain the values of the row – as key value pairs.</span></span> <span data-ttu-id="9792d-137">Mer information finns här: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span><span class="sxs-lookup"><span data-stu-id="9792d-137">More information is found here: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span></span>

### <a name="csdl---conceptual-schema-definition-language"></a><span data-ttu-id="9792d-138">CSDL - Conceptual Schema Definition Language</span><span class="sxs-lookup"><span data-stu-id="9792d-138">CSDL - Conceptual Schema Definition Language</span></span>
<span data-ttu-id="9792d-139">Här kan du definiera funktioner (SPROCs) och de entiteter som är tillgängliga via en databas.</span><span class="sxs-lookup"><span data-stu-id="9792d-139">Allows defining functions (SPROCs) and entities that are exposed through a database.</span></span> <span data-ttu-id="9792d-140">Mer information finns här: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span><span class="sxs-lookup"><span data-stu-id="9792d-140">More information found here: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span></span>  

> [!TIP]
> <span data-ttu-id="9792d-141">Klicka på den **andra versioner** listrutan och väljer en version om du inte ser artikeln.</span><span class="sxs-lookup"><span data-stu-id="9792d-141">Click the **other versions** dropdown and select a version if you don’t see the article.</span></span>
> 
> 

### <a name="edm---entry-data-model"></a><span data-ttu-id="9792d-142">EDM - post-datamodell</span><span class="sxs-lookup"><span data-stu-id="9792d-142">EDM - Entry Data Model</span></span>
* <span data-ttu-id="9792d-143">Översikt: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]</span><span class="sxs-lookup"><span data-stu-id="9792d-143">Overview: [http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx][OverviewLink]</span></span>

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* <span data-ttu-id="9792d-144">Förhandsversion: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]</span><span class="sxs-lookup"><span data-stu-id="9792d-144">Preview: [http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx][PreviewLink]</span></span>

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* <span data-ttu-id="9792d-145">Datatyper: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]</span><span class="sxs-lookup"><span data-stu-id="9792d-145">Data types: [http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx][DataTypesLink]</span></span>

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

<span data-ttu-id="9792d-146">I följande visas detaljerad vänster till höger som flödar från där klienten anger OData-uttryck (anrop till innehåll provider-webbtjänsten) att hämta Resultatdata tillbaka:</span><span class="sxs-lookup"><span data-stu-id="9792d-146">The following shows the detailed Left to Right flow from where the client enters the OData statement (call to the content provider’s web service) to getting the results/data back:</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a><span data-ttu-id="9792d-148">CSDL-grunderna</span><span class="sxs-lookup"><span data-stu-id="9792d-148">CSDL Basics</span></span>
<span data-ttu-id="9792d-149">En CSDL (konceptuellt Schema Definition Language) är en specifikation som definierar hur att beskriva webbtjänst eller database-tjänsten i vanliga XML ordflöde på Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9792d-149">A CSDL (Conceptual Schema Definition Language) is a specification defining how to describe web service or database service in common XML verbiage to the Azure Marketplace.</span></span> <span data-ttu-id="9792d-150">CSDL beskriver de viktiga delar som **möjliggör överföring av data från datakällan på Azure Marketplace.**</span><span class="sxs-lookup"><span data-stu-id="9792d-150">CSDL describes the critical pieces that **makes the passing of data from the Data Source to the Azure Marketplace possible.**</span></span> <span data-ttu-id="9792d-151">Här beskrivs de viktigaste delarna:</span><span class="sxs-lookup"><span data-stu-id="9792d-151">The main pieces are described here:</span></span>

* <span data-ttu-id="9792d-152">Gränssnittsinformation som beskriver alla offentligt tillgängliga funktioner (FunctionImport nod)</span><span class="sxs-lookup"><span data-stu-id="9792d-152">Interface information describing all publicly available functions (FunctionImport Node)</span></span>
* <span data-ttu-id="9792d-153">Information om datatyp för alla requests(input) meddelandet och meddelandet responses(outputs) (EntityType-EntityContainer/EntitySet noder)</span><span class="sxs-lookup"><span data-stu-id="9792d-153">Data type information for all message requests(input) and message responses(outputs) (EntityContainer/EntitySet/EntityType Nodes)</span></span>
* <span data-ttu-id="9792d-154">Bindningsinformation om transportprotokollet ska användas (sidhuvud nod)</span><span class="sxs-lookup"><span data-stu-id="9792d-154">Binding information about the transport protocol to be used (Header Node)</span></span>
* <span data-ttu-id="9792d-155">Information om för att hitta den angivna tjänsten (BaseURI attribut)</span><span class="sxs-lookup"><span data-stu-id="9792d-155">Address information for locating the specified service (BaseURI attribute)</span></span>

<span data-ttu-id="9792d-156">I kort sagt kan representerar CSDL ett kontrakt och språk-plattformsoberoende mellan service begärande och tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="9792d-156">In a nutshell, the CSDL represents a platform- and language-independent contract between the service requestor and the service provider.</span></span> <span data-ttu-id="9792d-157">Med CSDL kan en klient Leta upp en webbtjänst för service-databasen och anropa en av dess offentligt tillgängliga funktioner.</span><span class="sxs-lookup"><span data-stu-id="9792d-157">Using the CSDL, a client can locate a web service/database service and invoke any of its publicly available functions.</span></span>

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a><span data-ttu-id="9792d-158">Om en CSDL till en databas eller en samling</span><span class="sxs-lookup"><span data-stu-id="9792d-158">Relating a CSDL to a Database or a Collection</span></span>
<span data-ttu-id="9792d-159">**Specifikationen CSDL**</span><span class="sxs-lookup"><span data-stu-id="9792d-159">**The CSDL Specification**</span></span>

<span data-ttu-id="9792d-160">CSDL är XML-grammatiken för att beskriva en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="9792d-160">CSDL is XML grammar for describing a web service.</span></span> <span data-ttu-id="9792d-161">Specifikationen själva är uppdelad i 4 viktiga element: EntitySet, FunctionImport; Namnområdet och EntityType.</span><span class="sxs-lookup"><span data-stu-id="9792d-161">The specification itself is divided into 4 major elements:  EntitySet, FunctionImport; NameSpace, and EntityType.</span></span>

<span data-ttu-id="9792d-162">Att göra denna framställning lättare att förstå kan en CSDL avser en tabell.</span><span class="sxs-lookup"><span data-stu-id="9792d-162">To make this abstraction easier to understand lets relate a CSDL to a table.</span></span>

<span data-ttu-id="9792d-163">Kom ihåg;</span><span class="sxs-lookup"><span data-stu-id="9792d-163">Remember;</span></span>

  <span data-ttu-id="9792d-164">CSDL representerar en plattform och språkoberoende avtal mellan den **service begärande** och **tjänstleverantör**.</span><span class="sxs-lookup"><span data-stu-id="9792d-164">CSDL represents a platform- and language-independent contract between the **service requestor** and the **service provider**.</span></span> <span data-ttu-id="9792d-165">Med hjälp av CSDL, en **klienten** kan hitta en **webbtjänsten tjänstdatabasen/** och anropa en av dess offentligt tillgängliga **funktioner.**</span><span class="sxs-lookup"><span data-stu-id="9792d-165">Using CSDL, a **client** can locate a **web service/database service** and invoke any of its publicly available **functions.**</span></span>

<span data-ttu-id="9792d-166">För en datatjänst kan fyra delar av en CSDL betraktas som en databas, tabeller, kolumner och Store-procedur.</span><span class="sxs-lookup"><span data-stu-id="9792d-166">For a Data Service the four parts of a CSDL can be thought of in terms of a Database, Table, Column, and Store Procedure.</span></span>

<span data-ttu-id="9792d-167">Om dessa på följande sätt för en datatjänst:</span><span class="sxs-lookup"><span data-stu-id="9792d-167">Relating these as follows for a Data Service:</span></span>

* <span data-ttu-id="9792d-168">EntityContainer ~ = databas</span><span class="sxs-lookup"><span data-stu-id="9792d-168">EntityContainer  ~=  Database</span></span>
* <span data-ttu-id="9792d-169">EntitySet ~ = tabell</span><span class="sxs-lookup"><span data-stu-id="9792d-169">EntitySet  ~=  Table</span></span>
* <span data-ttu-id="9792d-170">EntityType ~ = kolumner</span><span class="sxs-lookup"><span data-stu-id="9792d-170">EntityType  ~= Columns</span></span>
* <span data-ttu-id="9792d-171">FunctionImport ~ = lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="9792d-171">FunctionImport  ~=  Stored Procedure</span></span>

<span data-ttu-id="9792d-172">**HTTP-verb tillåts**</span><span class="sxs-lookup"><span data-stu-id="9792d-172">**HTTP Verbs allowed**</span></span>

* <span data-ttu-id="9792d-173">– Returnerar värden hämtas från db (returnerar en mängd)</span><span class="sxs-lookup"><span data-stu-id="9792d-173">GET – returns values from the db (returns a Collection)</span></span>
* <span data-ttu-id="9792d-174">POST – används för att skicka data till och valfria returvärden från db (skapa en ny post i samlingen, return-id eller-URI)</span><span class="sxs-lookup"><span data-stu-id="9792d-174">POST – used to pass data to and optional return values from the db (Create a new entry in the collection, return id/URI)</span></span>
* <span data-ttu-id="9792d-175">Ta bort – tar bort data från DB (tar bort en samling)</span><span class="sxs-lookup"><span data-stu-id="9792d-175">DELETE – Deletes data from the DB (Deletes a collection)</span></span>
* <span data-ttu-id="9792d-176">PLACERA – uppdatera data i en DB (ersätta en samling eller skapa ett)</span><span class="sxs-lookup"><span data-stu-id="9792d-176">PUT – Update data into a DB (replace a collection or create one)</span></span>

## <a name="metadatamapping-document"></a><span data-ttu-id="9792d-177">Metadata-mappning dokumentet</span><span class="sxs-lookup"><span data-stu-id="9792d-177">Metadata/Mapping Document</span></span>
<span data-ttu-id="9792d-178">Metadata-mappning dokumentet används för att mappa en innehållsleverantör befintliga webbtjänster så att den kan visas som en OData-webbtjänst i Azure Marketplace-systemet.</span><span class="sxs-lookup"><span data-stu-id="9792d-178">The metadata/mapping document is used to map a content provider’s existing web services so that it can be exposed as an OData web service by the Azure Marketplace system.</span></span> <span data-ttu-id="9792d-179">Den är baserad på CSDL och implementerar några tillägg till CSDL till passar REST baserat webbtjänsterna som exponeras via Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9792d-179">It is based on CSDL and implements a few extensions to CSDL to accommodate the needs of REST based web services exposed through Azure Marketplace.</span></span> <span data-ttu-id="9792d-180">Tillägg finns i den [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namnområde.</span><span class="sxs-lookup"><span data-stu-id="9792d-180">The extensions are found in the [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namespace.</span></span>

<span data-ttu-id="9792d-181">Ett exempel på CSDL följer: (kopiera och klistra in den nedan exempel CSDL till en XML-redigerare och ändrar så att den matchar din tjänst.</span><span class="sxs-lookup"><span data-stu-id="9792d-181">An example of the CSDL follows:  (Copy and paste the below example CSDL into an XML editor and change to match your Service.</span></span>  <span data-ttu-id="9792d-182">Klistra in i CSDL mappning DataService fliken när du skapar din tjänst i den [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).</span><span class="sxs-lookup"><span data-stu-id="9792d-182">Then paste into CSDL Mapping under DataService tab when creating your service in the  [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).</span></span>

<span data-ttu-id="9792d-183">**Villkor:** om CSDL villkor till den [Publiceringsportal](https://publish.windowsazure.com) Användargränssnittet (PPUI) villkor.</span><span class="sxs-lookup"><span data-stu-id="9792d-183">**Terms:** Relating the CSDL terms to the [Publishing Portal](https://publish.windowsazure.com) UI (PPUI) terms.</span></span>

* <span data-ttu-id="9792d-184">Erbjuder ”Title” i PPUI relaterar till MyWebOffer</span><span class="sxs-lookup"><span data-stu-id="9792d-184">Offer “Title” in the PPUI relates to MyWebOffer</span></span>
* <span data-ttu-id="9792d-185">Mitt företag i PPUI avser **Publisher visningsnamn** i den [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI</span><span class="sxs-lookup"><span data-stu-id="9792d-185">MyCompany in the PPUI relates to **Publisher Display Name** in the [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI</span></span>
* <span data-ttu-id="9792d-186">Din API är kopplat till en webbplats eller en Data-tjänsten (en Plan i PPUI)</span><span class="sxs-lookup"><span data-stu-id="9792d-186">Your API relates to a Web or Data Service (a Plan in the PPUI)</span></span>

<span data-ttu-id="9792d-187">**Hierarki:** ett företag (innehållsleverantören) äger erbjudanden som har plan eller de planer, nämligen service(s) vilken rad upp med ett API.</span><span class="sxs-lookup"><span data-stu-id="9792d-187">**Hierarchy:** A Company (Content Provider) owns Offer(s) which have Plan(s), namely service(s), which line up with an API.</span></span>

### <a name="webservice-csdl-example"></a><span data-ttu-id="9792d-188">Webbtjänsten CSDL-exempel</span><span class="sxs-lookup"><span data-stu-id="9792d-188">WebService CSDL Example</span></span>
<span data-ttu-id="9792d-189">Ansluter till en tjänst som visar en web application slutpunkt (t.ex. en C#-program)</span><span class="sxs-lookup"><span data-stu-id="9792d-189">Connects to a service that is exposing an web application endpoint (like a C# application)</span></span>

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
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
> <span data-ttu-id="9792d-190">Visa fler CSDL Web Service-exempel i artikeln [exempel för att matcha en befintlig webbtjänst för OData via CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span><span class="sxs-lookup"><span data-stu-id="9792d-190">View more CSDL Web Service examples in the article [Examples of mapping an existing web service to OData through CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span></span>
> 
> 

### <a name="dataservice-csdl-example"></a><span data-ttu-id="9792d-191">DataService CSDL-exempel</span><span class="sxs-lookup"><span data-stu-id="9792d-191">DataService CSDL Example</span></span>
<span data-ttu-id="9792d-192">Ansluter till en tjänst som Exponerar en databastabell eller vy som en slutpunkt som exemplet nedan visar två API: erna för grundläggande Data baserat API CSDL (kan använda vyer i stället för tabeller).</span><span class="sxs-lookup"><span data-stu-id="9792d-192">Connects to a service that is exposing a database table or view as an endpoint Below example shows two APIs for Data base based API CSDL (can use views rather than tables).</span></span>

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
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

## <a name="see-also"></a><span data-ttu-id="9792d-193">Se även</span><span class="sxs-lookup"><span data-stu-id="9792d-193">See Also</span></span>
* <span data-ttu-id="9792d-194">Om du är intresserad av att lära sig och förstå de specifika noderna och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.</span><span class="sxs-lookup"><span data-stu-id="9792d-194">If you are interested in learning and understanding the specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="9792d-195">Om du vill granska exempel den här artikeln [mappa Data Service OData-exempel](marketplace-publishing-data-service-creation-odata-mapping-examples.md) att exempelkoden förstå och kodsyntax och på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="9792d-195">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) to see sample code and understand code syntax and context.</span></span>
* <span data-ttu-id="9792d-196">Om du vill återgå till den föreskrivna sökvägen för att publicera en datatjänst på Azure Marketplace, den här artikeln [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="9792d-196">To return to the prescribed path for publishing a Data Service to the Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

