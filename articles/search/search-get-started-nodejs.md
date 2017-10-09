---
title: "aaaGet igång med Azure Search i Node.js | Microsoft Docs"
description: "Se hur du bygger ett sökprogram på en värd- och molnbaserad söktjänst i Azure med Node.js som programmeringsspråk."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: evboyle
ms.openlocfilehash: e9c7d756c2ea191ee2a285485c90439b96aa73b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a><span data-ttu-id="0f6e3-103">Komma igång med Azure Search i Node.js</span><span class="sxs-lookup"><span data-stu-id="0f6e3-103">Get started with Azure Search in Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f6e3-104">Portal</span><span class="sxs-lookup"><span data-stu-id="0f6e3-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="0f6e3-105">.NET</span><span class="sxs-lookup"><span data-stu-id="0f6e3-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="0f6e3-106">Lär dig hur toobuild anpassade Node.js söka program som använder Azure Search för dess sökinställningar.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-106">Learn how toobuild a custom Node.js search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="0f6e3-107">Den här kursen använder hello [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekt och åtgärder som används i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-107">This tutorial uses hello [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objects and operations used in this exercise.</span></span>

<span data-ttu-id="0f6e3-108">Vi använde [Node.js](https://Nodejs.org) och NPM, [Sublime Text 3](http://www.sublimetext.com/3), och Windows PowerShell på Windows 8.1 toodevelop och testa den här koden.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-108">We used [Node.js](https://Nodejs.org) and NPM, [Sublime Text 3](http://www.sublimetext.com/3), and Windows PowerShell on Windows 8.1 toodevelop and test this code.</span></span>

<span data-ttu-id="0f6e3-109">toorun det här exemplet måste du ha en Azure Search-tjänst som du kan registrera dig för i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0f6e3-109">toorun this sample, you must have an Azure Search service, which you can sign up for in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="0f6e3-110">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-110">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

## <a name="about-hello-data"></a><span data-ttu-id="0f6e3-111">Om hello data</span><span class="sxs-lookup"><span data-stu-id="0f6e3-111">About hello data</span></span>
<span data-ttu-id="0f6e3-112">Det här exempelprogrammet använder data från hello [USA geologiska tjänster (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrerad på hello tillstånd Rhode dataö tooreduce hello dataset storlek.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-112">This sample application uses data from hello [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on hello state of Rhode Island tooreduce hello dataset size.</span></span> <span data-ttu-id="0f6e3-113">Vi använder den här data toobuild ett sökprogram som returnerar Landmärke byggnader, till exempel sjukhus och skolorna samt geologiska funktioner som dataströmmar, sjöar och toppmöten.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-113">We'll use this data toobuild a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="0f6e3-114">I det här programmet hello **DataIndexer** programmet skapar och belastningar hello index med hjälp av en [indexeraren](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstruktion, hämtar hello filtrerade USGS datamängd från en offentlig Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-114">In this application, hello **DataIndexer** program builds and loads hello index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving hello filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="0f6e3-115">Autentiseringsuppgifter och anslutningen information toohello online datakällan har angetts i hello programkod.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-115">Credentials and connection information toohello online data source is provided in hello program code.</span></span> <span data-ttu-id="0f6e3-116">Ingen ytterligare konfiguration krävs.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-116">No further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="0f6e3-117">Vi har använt ett filter på den här datauppsättningen toostay under hello 10 000 dokumentet gränsen på hello kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-117">We applied a filter on this dataset toostay under hello 10,000 document limit of hello free pricing tier.</span></span> <span data-ttu-id="0f6e3-118">Den här begränsningen gäller inte om du använder hello standardnivån.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-118">If you use hello standard tier, this limit does not apply.</span></span> <span data-ttu-id="0f6e3-119">Mer information om kapaciteten för varje prisnivå finns i [Tjänstbegränsningar för Search](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="0f6e3-119">For details about capacity for each pricing tier, see [Search service limits](search-limits-quotas-capacity.md).</span></span>
> 
> 

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="0f6e3-120">Hitta hello tjänstnamn och api-nyckel för Azure Search-tjänst</span><span class="sxs-lookup"><span data-stu-id="0f6e3-120">Find hello service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="0f6e3-121">När du har skapat hello service Retur-URL för toohello portal tooget hello eller `api-key`.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-121">After you create hello service, return toohello portal tooget hello URL or `api-key`.</span></span> <span data-ttu-id="0f6e3-122">Anslutningar tooyour Search-tjänsten kräver att du har båda hello-URL: en och en `api-key` tooauthenticate hello anrop.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-122">Connections tooyour Search service require that you have both hello URL and an `api-key` tooauthenticate hello call.</span></span>

1. <span data-ttu-id="0f6e3-123">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0f6e3-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0f6e3-124">Klicka på hello hopp-fältet **söktjänsten** toolist alla Azure Search-tjänster som har etablerats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-124">In hello jump bar, click **Search service** toolist all Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="0f6e3-125">Välj hello-tjänster som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-125">Select hello service you want toouse.</span></span>
4. <span data-ttu-id="0f6e3-126">Du bör se paneler viktig information, till exempel hello nyckelikonen för att komma åt hello admin nycklar på hello service instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-126">On hello service dashboard, you should see tiles for essential information, such as hello key icon for accessing hello admin keys.</span></span>
5. <span data-ttu-id="0f6e3-127">Kopiera hello-tjänstens URL, admin-nyckel och en nyckel för frågan.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-127">Copy hello service URL, an admin key, and a query key.</span></span> <span data-ttu-id="0f6e3-128">Du måste alla tre senare när du lägger till dem toohello config.js-fil.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-128">You need all three later when you add them toohello config.js file.</span></span>

## <a name="download-hello-sample-files"></a><span data-ttu-id="0f6e3-129">Hämta hello exempelfilerna</span><span class="sxs-lookup"><span data-stu-id="0f6e3-129">Download hello sample files</span></span>
<span data-ttu-id="0f6e3-130">Använd någon av följande metoder toodownload hello exempel hello.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-130">Use either one of hello following approaches toodownload hello sample.</span></span>

1. <span data-ttu-id="0f6e3-131">Gå för[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span><span class="sxs-lookup"><span data-stu-id="0f6e3-131">Go too[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span></span>
2. <span data-ttu-id="0f6e3-132">Klicka på **hämta ZIP**, spara hello ZIP-fil och sedan extrahera alla hello filer den innehåller.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-132">Click **Download ZIP**, save hello .zip file, and then extract all hello files it contains.</span></span>

<span data-ttu-id="0f6e3-133">Alla efterföljande filändringar och körningsinstruktioner görs mot filer i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-133">All subsequent file modifications and run statements are made against files in this folder.</span></span>

## <a name="update-hello-configjs-with-your-search-service-url-and-api-key"></a><span data-ttu-id="0f6e3-134">Uppdatera hello config.js.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-134">Update hello config.js.</span></span> <span data-ttu-id="0f6e3-135">med Search-tjänstens URL och API-nyckel</span><span class="sxs-lookup"><span data-stu-id="0f6e3-135">with your Search service URL and api-key</span></span>
<span data-ttu-id="0f6e3-136">Med hjälp av hello URL: en och api-nyckel som du kopierade tidigare, ange hello URL, admin-nyckel och fråga nyckel i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-136">Using hello URL and api-key that you copied earlier, specify hello URL, admin-key, and query-key in configuration file.</span></span>

<span data-ttu-id="0f6e3-137">Administratörsnycklar beviljar fullständig kontroll över tjänståtgärder, inklusive att skapa eller ta bort ett index och inläsning av dokument.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-137">Admin keys grant full control over service operations, including creating or deleting an index and loading documents.</span></span> <span data-ttu-id="0f6e3-138">Frågan nycklar är däremot för skrivskyddade åtgärder, som vanligtvis används av klientprogram som ansluter tooAzure sökning.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-138">In contrast, query keys are for read-only operations, typically used by client applications that connect tooAzure Search.</span></span>

<span data-ttu-id="0f6e3-139">I det här exemplet innehåller vi hello fråga viktiga toohelp förstärka hello bästa praxis för att använda hello Frågenyckeln i klientprogram.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-139">In this sample, we include hello query key toohelp reinforce hello best practice of using hello query key in client applications.</span></span>

<span data-ttu-id="0f6e3-140">följande skärmbild visar hello **config.js** öppna i en textredigerare, med hello relevanta poster avgränsat så att du kan se där tooupdate hello-fil med hello värden som är giltiga för din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-140">hello following screenshot shows **config.js** open in a text editor, with hello relevant entries demarcated so that you can see where tooupdate hello file with hello values that are valid for your search service.</span></span>

![][5]

## <a name="host-a-runtime-environment-for-hello-sample"></a><span data-ttu-id="0f6e3-141">Värd för en körningsmiljö hello-exempel</span><span class="sxs-lookup"><span data-stu-id="0f6e3-141">Host a runtime environment for hello sample</span></span>
<span data-ttu-id="0f6e3-142">hello urvalet kräver en HTTP-server som du kan installera globalt med npm.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-142">hello sample requires an HTTP server, which you can install globally using npm.</span></span>

<span data-ttu-id="0f6e3-143">Använda ett PowerShell-fönster för hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-143">Use a PowerShell window for hello following commands.</span></span>

1. <span data-ttu-id="0f6e3-144">Navigera toohello mapp som innehåller hello **package.json** fil.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-144">Navigate toohello folder that contains hello **package.json** file.</span></span>
2. <span data-ttu-id="0f6e3-145">Skriv `npm install`.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-145">Type `npm install`.</span></span>
3. <span data-ttu-id="0f6e3-146">Skriv `npm install -g http-server`.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-146">Type `npm install -g http-server`.</span></span>

## <a name="build-hello-index-and-run-hello-application"></a><span data-ttu-id="0f6e3-147">Skapa hello indexet och kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="0f6e3-147">Build hello index and run hello application</span></span>
1. <span data-ttu-id="0f6e3-148">Skriv `npm run indexDocuments`.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-148">Type `npm run indexDocuments`.</span></span>
2. <span data-ttu-id="0f6e3-149">Skriv `npm run build`.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-149">Type `npm run build`.</span></span>
3. <span data-ttu-id="0f6e3-150">Skriv `npm run start_server`.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-150">Type `npm run start_server`.</span></span>
4. <span data-ttu-id="0f6e3-151">Dirigera webbläsaren till `http://localhost:8080/index.html`</span><span class="sxs-lookup"><span data-stu-id="0f6e3-151">Direct your browser at `http://localhost:8080/index.html`</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="0f6e3-152">Söka i USGS-data</span><span class="sxs-lookup"><span data-stu-id="0f6e3-152">Search on USGS data</span></span>
<span data-ttu-id="0f6e3-153">Hej USGS datauppsättningen innehåller poster som är relevanta toohello tillstånd Rhode dataö.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-153">hello USGS data set includes records that are relevant toohello state of Rhode Island.</span></span> <span data-ttu-id="0f6e3-154">Om du klickar på **Sök** på en tom sökrutan visas hello upp 50 poster som är hello standard.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-154">If you click **Search** on an empty search box, you get hello top 50 entries, which is hello default.</span></span>

<span data-ttu-id="0f6e3-155">Anger en sökterm blir hello sökmotor något toogo på.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-155">Entering a search term gives hello search engine something toogo on.</span></span> <span data-ttu-id="0f6e3-156">Prova att skriva namnet på någon från regionen.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-156">Try entering a regional name.</span></span> <span data-ttu-id="0f6e3-157">”Roger Williams” var hello första resursstyrningen av Rhode dataö.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-157">"Roger Williams" was hello first governor of Rhode Island.</span></span> <span data-ttu-id="0f6e3-158">Många parker, byggnader och skolor bär hans namn.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-158">Numerous parks, buildings, and schools are named after him.</span></span>

![][9]

<span data-ttu-id="0f6e3-159">Du kan också prova någon av dessa söktermer:</span><span class="sxs-lookup"><span data-stu-id="0f6e3-159">You could also try any of these terms:</span></span>

* <span data-ttu-id="0f6e3-160">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="0f6e3-160">Pawtucket</span></span>
* <span data-ttu-id="0f6e3-161">Pembroke</span><span class="sxs-lookup"><span data-stu-id="0f6e3-161">Pembroke</span></span>
* <span data-ttu-id="0f6e3-162">goose +cape</span><span class="sxs-lookup"><span data-stu-id="0f6e3-162">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f6e3-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f6e3-163">Next steps</span></span>
<span data-ttu-id="0f6e3-164">Detta är hello första Azure Search-självstudierna baserat på Node.js och hello USGS dataset.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-164">This is hello first Azure Search tutorial based on Node.js and hello USGS dataset.</span></span> <span data-ttu-id="0f6e3-165">Över tiden, kommer vi utöka den här självstudiekursen toodemonstrate ytterligare sökfunktioner som du kanske vill toouse i din anpassade lösningar.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-165">Over time, we'll extend this tutorial toodemonstrate additional search features you might want toouse in your custom solutions.</span></span>

<span data-ttu-id="0f6e3-166">Om du redan har viss erfarenhet av Azure Search kan du använda det här exemplet som utgångspunkt för att prova förslagsställare (frågeifyllningsförslag eller Komplettera automatiskt), filter och aspektbaserad navigering.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-166">If you already have some background in Azure Search, you can use this sample as a springboard for trying suggesters (type-ahead or autocomplete queries), filters, and faceted navigation.</span></span> <span data-ttu-id="0f6e3-167">Du kan också förbättra vid hello sökresultatsidan genom att lägga till antal och batchbearbetning dokument så att användare kan bläddra igenom hello resultat.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-167">You can also improve upon hello search results page by adding counts and batching documents so that users can page through hello results.</span></span>

<span data-ttu-id="0f6e3-168">Nya tooAzure sökningen?</span><span class="sxs-lookup"><span data-stu-id="0f6e3-168">New tooAzure Search?</span></span> <span data-ttu-id="0f6e3-169">Vi rekommenderar att du försöker andra självstudiekurser toodevelop förstå vad du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-169">We recommend trying other tutorials toodevelop an understanding of what you can create.</span></span> <span data-ttu-id="0f6e3-170">Besök vår [dokumentationssidan](https://azure.microsoft.com/documentation/services/search/) toofind mer resurser.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-170">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) toofind more resources.</span></span> <span data-ttu-id="0f6e3-171">Du kan också visa hello länkar i vår [Video-och kursen](search-video-demo-tutorial-list.md) tooaccess mer information.</span><span class="sxs-lookup"><span data-stu-id="0f6e3-171">You can also view hello links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) tooaccess more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
