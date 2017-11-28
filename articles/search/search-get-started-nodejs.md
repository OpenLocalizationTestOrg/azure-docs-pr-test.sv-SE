---
title: "Komma igång med Azure Search i Node.js | Microsoft Docs"
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
ms.openlocfilehash: 32865ed986f5eea961ef2c3813dcc6531498c90a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a><span data-ttu-id="cddd3-103">Komma igång med Azure Search i Node.js</span><span class="sxs-lookup"><span data-stu-id="cddd3-103">Get started with Azure Search in Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cddd3-104">Portal</span><span class="sxs-lookup"><span data-stu-id="cddd3-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="cddd3-105">.NET</span><span class="sxs-lookup"><span data-stu-id="cddd3-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="cddd3-106">Lär dig hur du skapar ett anpassat Node.js-sökprogram som använder Azure Search som sökmiljö.</span><span class="sxs-lookup"><span data-stu-id="cddd3-106">Learn how to build a custom Node.js search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="cddd3-107">I den här självstudiekursen används [REST-API:et för tjänsten Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) för att skapa de objekt och åtgärder som används i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="cddd3-107">This tutorial uses the [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) to construct the objects and operations used in this exercise.</span></span>

<span data-ttu-id="cddd3-108">Vi använde [Node.js](https://Nodejs.org) och NPM, [Sublime Text 3](http://www.sublimetext.com/3) och Windows PowerShell i Windows 8.1 när vi utvecklade och testade den här koden.</span><span class="sxs-lookup"><span data-stu-id="cddd3-108">We used [Node.js](https://Nodejs.org) and NPM, [Sublime Text 3](http://www.sublimetext.com/3), and Windows PowerShell on Windows 8.1 to develop and test this code.</span></span>

<span data-ttu-id="cddd3-109">Om du vill köra det här exemplet måste du ha en Azure Search-tjänst, som du kan registrera dig för på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cddd3-109">To run this sample, you must have an Azure Search service, which you can sign up for in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="cddd3-110">Stegvisa instruktioner finns i [Skapa en Azure Search-tjänst på portalen](search-create-service-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cddd3-110">See [Create an Azure Search service in the portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

## <a name="about-the-data"></a><span data-ttu-id="cddd3-111">Om de data som används</span><span class="sxs-lookup"><span data-stu-id="cddd3-111">About the data</span></span>
<span data-ttu-id="cddd3-112">Det här exempelprogrammet använder data från [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), som har filtrerats på delstaten Rhode Island för att minska datauppsättningens storlek.</span><span class="sxs-lookup"><span data-stu-id="cddd3-112">This sample application uses data from the [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on the state of Rhode Island to reduce the dataset size.</span></span> <span data-ttu-id="cddd3-113">Vi ska använda dessa data för att skapa ett sökprogram som returnerar viktiga byggnader som sjukhus och skolor, samt geologiska element som vattendrag, sjöar och bergstoppar.</span><span class="sxs-lookup"><span data-stu-id="cddd3-113">We'll use this data to build a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="cddd3-114">I det här programmet bygger och läser programmet **DataIndexer** in indexet med hjälp av en [indexeringskonstruktion](https://msdn.microsoft.com/library/azure/dn798918.aspx) och hämtar den filtrerade USGS-datauppsättningen från en offentlig Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="cddd3-114">In this application, the **DataIndexer** program builds and loads the index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving the filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="cddd3-115">Autentiseringsuppgifter och anslutningsinformation för onlinedatakällan finns i programkoden.</span><span class="sxs-lookup"><span data-stu-id="cddd3-115">Credentials and connection information to the online data source is provided in the program code.</span></span> <span data-ttu-id="cddd3-116">Ingen ytterligare konfiguration krävs.</span><span class="sxs-lookup"><span data-stu-id="cddd3-116">No further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="cddd3-117">Vi har använt ett filter för den här datauppsättningen för att hålla oss under gränsen på 10 000 dokument för den kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="cddd3-117">We applied a filter on this dataset to stay under the 10,000 document limit of the free pricing tier.</span></span> <span data-ttu-id="cddd3-118">Den här begränsningen gäller inte om du använder standardnivån.</span><span class="sxs-lookup"><span data-stu-id="cddd3-118">If you use the standard tier, this limit does not apply.</span></span> <span data-ttu-id="cddd3-119">Mer information om kapaciteten för varje prisnivå finns i [Tjänstbegränsningar för Search](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="cddd3-119">For details about capacity for each pricing tier, see [Search service limits](search-limits-quotas-capacity.md).</span></span>
> 
> 

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="cddd3-120">Leta upp tjänstnamnet och API-nyckeln för Azure Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="cddd3-120">Find the service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="cddd3-121">När du har skapat tjänsten går du tillbaka till portalen för att hämta URL:en eller `api-key`.</span><span class="sxs-lookup"><span data-stu-id="cddd3-121">After you create the service, return to the portal to get the URL or `api-key`.</span></span> <span data-ttu-id="cddd3-122">Anslutningar till Search-tjänsten kräver att du har båda URL:en och en `api-key` för att autentisera anropet.</span><span class="sxs-lookup"><span data-stu-id="cddd3-122">Connections to your Search service require that you have both the URL and an `api-key` to authenticate the call.</span></span>

1. <span data-ttu-id="cddd3-123">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cddd3-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cddd3-124">I snabbåtkomstfältet klickar du på **Söktjänst** för att visa en lista över Azure Search-tjänsterna som har etablerats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cddd3-124">In the jump bar, click **Search service** to list all Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="cddd3-125">Markera den tjänst som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="cddd3-125">Select the service you want to use.</span></span>
4. <span data-ttu-id="cddd3-126">På instrumentpanelen för tjänsten ser du paneler för viktig information som nyckelikonen för att komma åt administatörsnycklarna.</span><span class="sxs-lookup"><span data-stu-id="cddd3-126">On the service dashboard, you should see tiles for essential information, such as the key icon for accessing the admin keys.</span></span>
5. <span data-ttu-id="cddd3-127">Kopiera tjänstens URL, en administratörsnyckel och en frågenyckel.</span><span class="sxs-lookup"><span data-stu-id="cddd3-127">Copy the service URL, an admin key, and a query key.</span></span> <span data-ttu-id="cddd3-128">Du behöver alla tre senare när du lägger till dem i filen config.js.</span><span class="sxs-lookup"><span data-stu-id="cddd3-128">You need all three later when you add them to the config.js file.</span></span>

## <a name="download-the-sample-files"></a><span data-ttu-id="cddd3-129">Ladda ned exempelfilerna</span><span class="sxs-lookup"><span data-stu-id="cddd3-129">Download the sample files</span></span>
<span data-ttu-id="cddd3-130">Använd någon av följande metoder för att hämta exemplet.</span><span class="sxs-lookup"><span data-stu-id="cddd3-130">Use either one of the following approaches to download the sample.</span></span>

1. <span data-ttu-id="cddd3-131">Gå till [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span><span class="sxs-lookup"><span data-stu-id="cddd3-131">Go to [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).</span></span>
2. <span data-ttu-id="cddd3-132">Klicka på **Ladda ned ZIP**, spara ZIP-filen och extrahera sedan alla filer som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="cddd3-132">Click **Download ZIP**, save the .zip file, and then extract all the files it contains.</span></span>

<span data-ttu-id="cddd3-133">Alla efterföljande filändringar och körningsinstruktioner görs mot filer i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="cddd3-133">All subsequent file modifications and run statements are made against files in this folder.</span></span>

## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a><span data-ttu-id="cddd3-134">Uppdatera config.js.</span><span class="sxs-lookup"><span data-stu-id="cddd3-134">Update the config.js.</span></span> <span data-ttu-id="cddd3-135">med Search-tjänstens URL och API-nyckel</span><span class="sxs-lookup"><span data-stu-id="cddd3-135">with your Search service URL and api-key</span></span>
<span data-ttu-id="cddd3-136">Använd URL:en och API-nyckeln som du kopierade tidigare, ange URL:en, administratörsnyckeln och frågenyckeln i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="cddd3-136">Using the URL and api-key that you copied earlier, specify the URL, admin-key, and query-key in configuration file.</span></span>

<span data-ttu-id="cddd3-137">Administratörsnycklar beviljar fullständig kontroll över tjänståtgärder, inklusive att skapa eller ta bort ett index och inläsning av dokument.</span><span class="sxs-lookup"><span data-stu-id="cddd3-137">Admin keys grant full control over service operations, including creating or deleting an index and loading documents.</span></span> <span data-ttu-id="cddd3-138">Frågenycklar är däremot avsedda för skrivskyddade åtgärder, som vanligtvis används av klientprogram som ansluter till Azure Search.</span><span class="sxs-lookup"><span data-stu-id="cddd3-138">In contrast, query keys are for read-only operations, typically used by client applications that connect to Azure Search.</span></span>

<span data-ttu-id="cddd3-139">I det här exemplet använder vi frågenyckeln för att uppfylla bästa praxis som rekommenderar att frågenyckeln används i klientprogram.</span><span class="sxs-lookup"><span data-stu-id="cddd3-139">In this sample, we include the query key to help reinforce the best practice of using the query key in client applications.</span></span>

<span data-ttu-id="cddd3-140">Följande skärmbild visar **config.js** i en textredigerare, med relevanta poster markerade så att du ser var du ska uppdatera filen med värdena för din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="cddd3-140">The following screenshot shows **config.js** open in a text editor, with the relevant entries demarcated so that you can see where to update the file with the values that are valid for your search service.</span></span>

![][5]

## <a name="host-a-runtime-environment-for-the-sample"></a><span data-ttu-id="cddd3-141">Värd för en körningsmiljö för exemplet</span><span class="sxs-lookup"><span data-stu-id="cddd3-141">Host a runtime environment for the sample</span></span>
<span data-ttu-id="cddd3-142">Exempelfilen kräver en HTTP-server, som du kan installera globalt med npm.</span><span class="sxs-lookup"><span data-stu-id="cddd3-142">The sample requires an HTTP server, which you can install globally using npm.</span></span>

<span data-ttu-id="cddd3-143">Använd ett PowerShell-fönster för följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="cddd3-143">Use a PowerShell window for the following commands.</span></span>

1. <span data-ttu-id="cddd3-144">Navigera till mappen som innehåller filen **package.json**.</span><span class="sxs-lookup"><span data-stu-id="cddd3-144">Navigate to the folder that contains the **package.json** file.</span></span>
2. <span data-ttu-id="cddd3-145">Skriv `npm install`.</span><span class="sxs-lookup"><span data-stu-id="cddd3-145">Type `npm install`.</span></span>
3. <span data-ttu-id="cddd3-146">Skriv `npm install -g http-server`.</span><span class="sxs-lookup"><span data-stu-id="cddd3-146">Type `npm install -g http-server`.</span></span>

## <a name="build-the-index-and-run-the-application"></a><span data-ttu-id="cddd3-147">Skapa indexet och kör programmet</span><span class="sxs-lookup"><span data-stu-id="cddd3-147">Build the index and run the application</span></span>
1. <span data-ttu-id="cddd3-148">Skriv `npm run indexDocuments`.</span><span class="sxs-lookup"><span data-stu-id="cddd3-148">Type `npm run indexDocuments`.</span></span>
2. <span data-ttu-id="cddd3-149">Skriv `npm run build`.</span><span class="sxs-lookup"><span data-stu-id="cddd3-149">Type `npm run build`.</span></span>
3. <span data-ttu-id="cddd3-150">Skriv `npm run start_server`.</span><span class="sxs-lookup"><span data-stu-id="cddd3-150">Type `npm run start_server`.</span></span>
4. <span data-ttu-id="cddd3-151">Dirigera webbläsaren till `http://localhost:8080/index.html`</span><span class="sxs-lookup"><span data-stu-id="cddd3-151">Direct your browser at `http://localhost:8080/index.html`</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="cddd3-152">Söka i USGS-data</span><span class="sxs-lookup"><span data-stu-id="cddd3-152">Search on USGS data</span></span>
<span data-ttu-id="cddd3-153">USGS-datauppsättningen innehåller poster som är relevanta för delstaten Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="cddd3-153">The USGS data set includes records that are relevant to the state of Rhode Island.</span></span> <span data-ttu-id="cddd3-154">Om du klickar på **Search** i en tom sökruta returneras 50 poster, vilket är standard.</span><span class="sxs-lookup"><span data-stu-id="cddd3-154">If you click **Search** on an empty search box, you get the top 50 entries, which is the default.</span></span>

<span data-ttu-id="cddd3-155">Om du skriver en sökterm ger du sökmotorn något att gå på.</span><span class="sxs-lookup"><span data-stu-id="cddd3-155">Entering a search term gives the search engine something to go on.</span></span> <span data-ttu-id="cddd3-156">Prova att skriva namnet på någon från regionen.</span><span class="sxs-lookup"><span data-stu-id="cddd3-156">Try entering a regional name.</span></span> <span data-ttu-id="cddd3-157">”Roger Williams” var Rhode Islands första guvernör.</span><span class="sxs-lookup"><span data-stu-id="cddd3-157">"Roger Williams" was the first governor of Rhode Island.</span></span> <span data-ttu-id="cddd3-158">Många parker, byggnader och skolor bär hans namn.</span><span class="sxs-lookup"><span data-stu-id="cddd3-158">Numerous parks, buildings, and schools are named after him.</span></span>

![][9]

<span data-ttu-id="cddd3-159">Du kan också prova någon av dessa söktermer:</span><span class="sxs-lookup"><span data-stu-id="cddd3-159">You could also try any of these terms:</span></span>

* <span data-ttu-id="cddd3-160">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="cddd3-160">Pawtucket</span></span>
* <span data-ttu-id="cddd3-161">Pembroke</span><span class="sxs-lookup"><span data-stu-id="cddd3-161">Pembroke</span></span>
* <span data-ttu-id="cddd3-162">goose +cape</span><span class="sxs-lookup"><span data-stu-id="cddd3-162">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="cddd3-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cddd3-163">Next steps</span></span>
<span data-ttu-id="cddd3-164">Det här är den första Azure Search-självstudiekursen som baseras på Node.js och USGS-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="cddd3-164">This is the first Azure Search tutorial based on Node.js and the USGS dataset.</span></span> <span data-ttu-id="cddd3-165">Med tiden kommer vi att utöka den här självstudiekursen och demonstrera ytterligare sökfunktioner som du kanske vill använda i dina anpassade lösningar.</span><span class="sxs-lookup"><span data-stu-id="cddd3-165">Over time, we'll extend this tutorial to demonstrate additional search features you might want to use in your custom solutions.</span></span>

<span data-ttu-id="cddd3-166">Om du redan har viss erfarenhet av Azure Search kan du använda det här exemplet som utgångspunkt för att prova förslagsställare (frågeifyllningsförslag eller Komplettera automatiskt), filter och aspektbaserad navigering.</span><span class="sxs-lookup"><span data-stu-id="cddd3-166">If you already have some background in Azure Search, you can use this sample as a springboard for trying suggesters (type-ahead or autocomplete queries), filters, and faceted navigation.</span></span> <span data-ttu-id="cddd3-167">Du kan även förbättra sidan med sökresultat genom att lägga till antal och batchbearbeta dokument så att användarna kan bläddra igenom resultaten.</span><span class="sxs-lookup"><span data-stu-id="cddd3-167">You can also improve upon the search results page by adding counts and batching documents so that users can page through the results.</span></span>

<span data-ttu-id="cddd3-168">Har du inte provat Azure Search än?</span><span class="sxs-lookup"><span data-stu-id="cddd3-168">New to Azure Search?</span></span> <span data-ttu-id="cddd3-169">Vi rekommenderar att du går andra självstudiekurser så att du ser vad du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="cddd3-169">We recommend trying other tutorials to develop an understanding of what you can create.</span></span> <span data-ttu-id="cddd3-170">Vår [dokumentationssida](https://azure.microsoft.com/documentation/services/search/) innehåller fler resurser.</span><span class="sxs-lookup"><span data-stu-id="cddd3-170">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) to find more resources.</span></span> <span data-ttu-id="cddd3-171">Mer information finns också på länkarna i [listan med videoklipp och självstudiekurser](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="cddd3-171">You can also view the links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) to access more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
