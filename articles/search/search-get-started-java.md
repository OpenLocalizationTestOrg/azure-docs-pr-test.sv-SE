---
title: "aaaGet igång med Azure Search i Java | Microsoft Docs"
description: "Hur toobuild ett värdbaserat moln Sök program på Azure med hjälp av Java som du programmeringsspråk."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: 5476a2103f3b60fe6ec78ff3d3fdba9fcff55c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-java"></a><span data-ttu-id="97398-103">Komma igång med Azure Search i Java</span><span class="sxs-lookup"><span data-stu-id="97398-103">Get started with Azure Search in Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="97398-104">Portal</span><span class="sxs-lookup"><span data-stu-id="97398-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="97398-105">.NET</span><span class="sxs-lookup"><span data-stu-id="97398-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="97398-106">Lär dig hur toobuild en anpassad Java söka program som använder Azure Search för dess sökinställningar.</span><span class="sxs-lookup"><span data-stu-id="97398-106">Learn how toobuild a custom Java search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="97398-107">Den här kursen använder hello [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekt och åtgärder som används i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="97398-107">This tutorial uses hello [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objects and operations used in this exercise.</span></span>

<span data-ttu-id="97398-108">toorun det här exemplet måste du ha en Azure Search-tjänst som du kan registrera dig för i hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="97398-108">toorun this sample, you must have an Azure Search service, which you can sign up for in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="97398-109">Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="97398-109">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

<span data-ttu-id="97398-110">Vi används hello följande programvara toobuild och testa det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="97398-110">We used hello following software toobuild and test this sample:</span></span>

* <span data-ttu-id="97398-111">[Eclipse IDE för Java EE-utvecklare](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span><span class="sxs-lookup"><span data-stu-id="97398-111">[Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span></span> <span data-ttu-id="97398-112">Vara att toodownload hello EE version.</span><span class="sxs-lookup"><span data-stu-id="97398-112">Be sure toodownload hello EE version.</span></span> <span data-ttu-id="97398-113">En av hello verifieringssteg kräver en funktion som bara finns i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="97398-113">One of hello verification steps requires a feature that is found only in this edition.</span></span>
* [<span data-ttu-id="97398-114">JDK 8u40</span><span class="sxs-lookup"><span data-stu-id="97398-114">JDK 8u40</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [<span data-ttu-id="97398-115">Apache Tomcat 8.0</span><span class="sxs-lookup"><span data-stu-id="97398-115">Apache Tomcat 8.0</span></span>](http://tomcat.apache.org/download-80.cgi)

## <a name="about-hello-data"></a><span data-ttu-id="97398-116">Om hello data</span><span class="sxs-lookup"><span data-stu-id="97398-116">About hello data</span></span>
<span data-ttu-id="97398-117">Det här exempelprogrammet använder data från hello [USA geologiska tjänster (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrerad på hello tillstånd Rhode dataö tooreduce hello dataset storlek.</span><span class="sxs-lookup"><span data-stu-id="97398-117">This sample application uses data from hello [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on hello state of Rhode Island tooreduce hello dataset size.</span></span> <span data-ttu-id="97398-118">Vi använder den här data toobuild ett sökprogram som returnerar Landmärke byggnader, till exempel sjukhus och skolorna samt geologiska funktioner som dataströmmar, sjöar och toppmöten.</span><span class="sxs-lookup"><span data-stu-id="97398-118">We'll use this data toobuild a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="97398-119">I det här programmet hello **SearchServlet.java** programmet skapar och belastningar hello index med hjälp av en [indexeraren](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstruktion, hämtar hello filtrerade USGS datamängd från en offentlig Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="97398-119">In this application, hello **SearchServlet.java** program builds and loads hello index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving hello filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="97398-120">Fördefinierade autentiseringsuppgifter och anslutningen information toohello online-datakälla finns i hello programkod.</span><span class="sxs-lookup"><span data-stu-id="97398-120">Predefined credentials and connection  information toohello online data source are provided in hello program code.</span></span> <span data-ttu-id="97398-121">Ingen ytterligare konfiguration krävs vad gäller dataåtkomsten.</span><span class="sxs-lookup"><span data-stu-id="97398-121">In terms of data access, no further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="97398-122">Vi har använt ett filter på den här datauppsättningen toostay under hello 10 000 dokumentet gränsen på hello kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="97398-122">We applied a filter on this dataset toostay under hello 10,000 document limit of hello free pricing tier.</span></span> <span data-ttu-id="97398-123">Den här begränsningen gäller inte om du använder hello standardnivån och du kan ändra den här koden toouse en större datamängd.</span><span class="sxs-lookup"><span data-stu-id="97398-123">If you use hello standard tier, this limit does not apply, and you can modify this code toouse a bigger dataset.</span></span> <span data-ttu-id="97398-124">Mer information om kapaciteten för varje prisnivå finns i [Gränser och begränsningar](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="97398-124">For details about capacity for each pricing tier, see [Limits and constraints](search-limits-quotas-capacity.md).</span></span>
> 
> 

## <a name="about-hello-program-files"></a><span data-ttu-id="97398-125">Om hello programfiler</span><span class="sxs-lookup"><span data-stu-id="97398-125">About hello program files</span></span>
<span data-ttu-id="97398-126">hello beskrivs följande lista hello-filer som är relevanta toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="97398-126">hello following list describes hello files that are relevant toothis sample.</span></span>

* <span data-ttu-id="97398-127">Search.JSP: Innehåller hello användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="97398-127">Search.jsp: Provides hello user interface</span></span>
* <span data-ttu-id="97398-128">SearchServlet.java: Tillhandahåller metoder (liknande tooa styrenhet i MVC)</span><span class="sxs-lookup"><span data-stu-id="97398-128">SearchServlet.java: Provides methods (similar tooa controller in MVC)</span></span>
* <span data-ttu-id="97398-129">SearchServiceClient.java: Hanterar HTTP-begäranden</span><span class="sxs-lookup"><span data-stu-id="97398-129">SearchServiceClient.java: Handles HTTP requests</span></span>
* <span data-ttu-id="97398-130">SearchServiceHelper.java: En hjälparklass som tillhandahåller statiska metoder</span><span class="sxs-lookup"><span data-stu-id="97398-130">SearchServiceHelper.java: A helper class that provides static methods</span></span>
* <span data-ttu-id="97398-131">Document.Java: Ger hello-datamodell</span><span class="sxs-lookup"><span data-stu-id="97398-131">Document.java: Provides hello data model</span></span>
* <span data-ttu-id="97398-132">Config.Properties: Anger hello Sök URL: en och api-nyckel</span><span class="sxs-lookup"><span data-stu-id="97398-132">config.properties: Sets hello Search service URL and api-key</span></span>
* <span data-ttu-id="97398-133">Pom.XML: Ett Maven-beroende</span><span class="sxs-lookup"><span data-stu-id="97398-133">Pom.xml: A Maven dependency</span></span>

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="97398-134">Hitta hello tjänstnamn och api-nyckel för Azure Search-tjänst</span><span class="sxs-lookup"><span data-stu-id="97398-134">Find hello service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="97398-135">REST API-anrop i Azure Search kräva att du anger hello URL: en och en api-nyckel.</span><span class="sxs-lookup"><span data-stu-id="97398-135">All REST API calls into Azure Search require that you provide hello service URL and an api-key.</span></span> 

1. <span data-ttu-id="97398-136">Logga in toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="97398-136">Sign in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="97398-137">Klicka på hello hopp-fältet **söktjänsten** toolist alla hello Azure Search-tjänster har etablerats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="97398-137">In hello jump bar, click **Search service** toolist all of hello Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="97398-138">Välj hello-tjänster som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="97398-138">Select hello service you want toouse.</span></span>
4. <span data-ttu-id="97398-139">På instrumentpanelen för hello-tjänsten ser du paneler för viktig information samt hello nyckelikonen för att komma åt hello admin nycklar.</span><span class="sxs-lookup"><span data-stu-id="97398-139">On hello service dashboard, you'll see tiles for essential information as well as hello key icon for accessing hello admin keys.</span></span>
   
      ![][3]
5. <span data-ttu-id="97398-140">Kopiera hello URL: en och en administrationsnyckel.</span><span class="sxs-lookup"><span data-stu-id="97398-140">Copy hello service URL and an admin key.</span></span> <span data-ttu-id="97398-141">Du behöver dem senare, när du lägger till dem toohello **config.properties** fil.</span><span class="sxs-lookup"><span data-stu-id="97398-141">You will need them later, when you add them toohello **config.properties** file.</span></span>

## <a name="download-hello-sample-files"></a><span data-ttu-id="97398-142">Hämta hello exempelfilerna</span><span class="sxs-lookup"><span data-stu-id="97398-142">Download hello sample files</span></span>
1. <span data-ttu-id="97398-143">Gå för[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="97398-143">Go too[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) on GitHub.</span></span>
2. <span data-ttu-id="97398-144">Klicka på **hämta ZIP**, spara hello ZIP-filen toodisk och sedan extrahera alla hello filer den innehåller.</span><span class="sxs-lookup"><span data-stu-id="97398-144">Click **Download ZIP**, save hello .zip file toodisk, and then extract all hello files it contains.</span></span> <span data-ttu-id="97398-145">Överväg att extrahera hello-filer i dina Java arbetsytan toomake som enklare toofind hello projektet senare.</span><span class="sxs-lookup"><span data-stu-id="97398-145">Consider extracting hello files into your Java workspace toomake it easier toofind hello project later.</span></span>
3. <span data-ttu-id="97398-146">hello exempelfiler är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="97398-146">hello sample files are read-only.</span></span> <span data-ttu-id="97398-147">Högerklicka på mappegenskaper och avmarkerar hello skrivskyddsattributet.</span><span class="sxs-lookup"><span data-stu-id="97398-147">Right-click folder properties and clear hello read-only attribute.</span></span>

<span data-ttu-id="97398-148">Alla efterföljande filändringar och körningsinstruktioner görs mot filer i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="97398-148">All subsequent file modifications and run statements will be made against files in this folder.</span></span>  

## <a name="import-project"></a><span data-ttu-id="97398-149">Importera projekt</span><span class="sxs-lookup"><span data-stu-id="97398-149">Import project</span></span>
1. <span data-ttu-id="97398-150">I Eclipse väljer du **File** > **Import** > **General** > **Existing Projects into Workspace**.</span><span class="sxs-lookup"><span data-stu-id="97398-150">In Eclipse, choose **File** > **Import** > **General** > **Existing Projects into Workspace**.</span></span>
   
    ![][4]
2. <span data-ttu-id="97398-151">I **väljer rotkatalogen**, bläddra toohello mapp som innehåller exempel på filer.</span><span class="sxs-lookup"><span data-stu-id="97398-151">In **Select root directory**, browse toohello folder containing sample files.</span></span> <span data-ttu-id="97398-152">Välj hello-mappen som innehåller hello .project mappen.</span><span class="sxs-lookup"><span data-stu-id="97398-152">Select hello folder that contains hello .project folder.</span></span> <span data-ttu-id="97398-153">hello projektet ska visas i hello **projekt** listan som det markerade objektet.</span><span class="sxs-lookup"><span data-stu-id="97398-153">hello project should appear in hello **Projects** list as a selected item.</span></span>
   
    ![][12]
3. <span data-ttu-id="97398-154">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="97398-154">Click **Finish**.</span></span>
4. <span data-ttu-id="97398-155">Använd **Projektutforskaren** tooview och redigera hello-filer.</span><span class="sxs-lookup"><span data-stu-id="97398-155">Use **Project Explorer** tooview and edit hello files.</span></span> <span data-ttu-id="97398-156">Om den inte redan är öppen klickar du på **fönstret** > **visa** > **Projektutforskaren** eller använda hello genvägen tooopen den.</span><span class="sxs-lookup"><span data-stu-id="97398-156">If it's not already open, click **Window** > **Show View** > **Project Explorer** or use hello shortcut tooopen it.</span></span>

## <a name="configure-hello-service-url-and-api-key"></a><span data-ttu-id="97398-157">Konfigurera hello URL: en och api-nyckel</span><span class="sxs-lookup"><span data-stu-id="97398-157">Configure hello service URL and api-key</span></span>
1. <span data-ttu-id="97398-158">I **Projektutforskaren**, dubbelklicka på **config.properties** tooedit hello konfigurationsinställningar som innehåller hello servernamn och api-nyckel.</span><span class="sxs-lookup"><span data-stu-id="97398-158">In **Project Explorer**, double-click **config.properties** tooedit hello configuration settings containing hello server name and api-key.</span></span>
2. <span data-ttu-id="97398-159">Se toohello anvisningarna tidigare i den här artikeln, där du hittade hello URL: en och api-nyckel i hello [Azure Portal](https://portal.azure.com), tooget hello värden som du kommer nu att träda i **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="97398-159">Refer toohello steps earlier in this article, where you found hello service URL and api-key in hello [Azure Portal](https://portal.azure.com), tooget hello values you will now enter into **config.properties**.</span></span>
3. <span data-ttu-id="97398-160">I **config.properties**, Ersätt ”Api-nyckeln” med hello api-nyckel för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="97398-160">In **config.properties**, replace "Api Key" with hello api-key for your service.</span></span> <span data-ttu-id="97398-161">Därefter tjänstnamn (hello första delen av hello URL http://servicename.search.windows.net) ersätter ”Tjänstenamn” i hello samma fil.</span><span class="sxs-lookup"><span data-stu-id="97398-161">Next, service name (hello first component of hello URL http://servicename.search.windows.net) replaces "service name" in hello same file.</span></span>
   
    ![][5]

## <a name="configure-hello-project-build-and-runtime-environments"></a><span data-ttu-id="97398-162">Konfigurera hello projekt, bygga och runtime-miljöer</span><span class="sxs-lookup"><span data-stu-id="97398-162">Configure hello project, build and runtime environments</span></span>
1. <span data-ttu-id="97398-163">Högerklicka på hello-projektet i Eclipse i Projektutforskaren > **egenskaper** > **Projektfasetter**.</span><span class="sxs-lookup"><span data-stu-id="97398-163">In Eclipse, in Project Explorer, right-click hello project > **Properties** > **Project Facets**.</span></span>
2. <span data-ttu-id="97398-164">Välj **Dynamic Web Module**, **Java** och **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="97398-164">Select **Dynamic Web Module**, **Java**, and **JavaScript**.</span></span>
   
    ![][6]
3. <span data-ttu-id="97398-165">Klicka på **Apply**.</span><span class="sxs-lookup"><span data-stu-id="97398-165">Click **Apply**.</span></span>
4. <span data-ttu-id="97398-166">Välj **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add**.</span><span class="sxs-lookup"><span data-stu-id="97398-166">Select **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add..**.</span></span>
5. <span data-ttu-id="97398-167">Visa Apache och välj hello version av hello Apache Tomcat-server som du tidigare har installerat.</span><span class="sxs-lookup"><span data-stu-id="97398-167">Expand Apache and select hello version of hello Apache Tomcat server you previously installed.</span></span> <span data-ttu-id="97398-168">I vårt system installerade vi version 8.</span><span class="sxs-lookup"><span data-stu-id="97398-168">On our system, we installed version 8.</span></span>
   
    ![][7]
6. <span data-ttu-id="97398-169">Ange hello Tomcat-installationskatalogen på hello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="97398-169">On hello next page, specify hello Tomcat installation directory.</span></span> <span data-ttu-id="97398-170">På en Windows-dator är detta antagligen C:\Program\Apache Software Foundation\Tomcat *version*.</span><span class="sxs-lookup"><span data-stu-id="97398-170">On a Windows computer, this will most likely be C:\Program Files\Apache Software Foundation\Tomcat *version*.</span></span>
7. <span data-ttu-id="97398-171">Klicka på **Finish**.</span><span class="sxs-lookup"><span data-stu-id="97398-171">Click **Finish**.</span></span>
8. <span data-ttu-id="97398-172">Välj **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span><span class="sxs-lookup"><span data-stu-id="97398-172">Select **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span></span>
9. <span data-ttu-id="97398-173">Välj **Standard VM**i **Add JRE**.</span><span class="sxs-lookup"><span data-stu-id="97398-173">In **Add JRE**, select **Standard VM**.</span></span>
10. <span data-ttu-id="97398-174">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="97398-174">Click **Next**.</span></span>
11. <span data-ttu-id="97398-175">Klicka på **Directory** i JRE Definition på JRE-startsidan.</span><span class="sxs-lookup"><span data-stu-id="97398-175">In JRE Definition, in JRE home, click **Directory**.</span></span>
12. <span data-ttu-id="97398-176">Navigera för**programfiler** > **Java** och välj hello JDK som du tidigare har installerat.</span><span class="sxs-lookup"><span data-stu-id="97398-176">Navigate too**Program Files** > **Java** and select hello JDK you previously installed.</span></span> <span data-ttu-id="97398-177">Det är viktigt tooselect hello JDK som hello JRE.</span><span class="sxs-lookup"><span data-stu-id="97398-177">It's important tooselect hello JDK as hello JRE.</span></span>
13. <span data-ttu-id="97398-178">Välj hello i installerat JREs **JDK**.</span><span class="sxs-lookup"><span data-stu-id="97398-178">In Installed JREs, choose hello **JDK**.</span></span> <span data-ttu-id="97398-179">Inställningarna bör se ut ungefär toohello följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="97398-179">Your settings should look similar toohello following screenshot.</span></span>
    
    ![][9]
14. <span data-ttu-id="97398-180">Alternativt, Välj **fönstret** > **webbläsare** > **Internet Explorer** tooopen hello programmet i en extern webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="97398-180">Optionally, select **Window** > **Web Browser** > **Internet Explorer** tooopen hello application in an external browser window.</span></span> <span data-ttu-id="97398-181">En extern webbläsare ger dig en bättre webbupplevelse.</span><span class="sxs-lookup"><span data-stu-id="97398-181">Using an external browser gives you a better Web application experience.</span></span>
    
    ![][8]

<span data-ttu-id="97398-182">Du har nu slutfört hello konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="97398-182">You have now completed hello configuration tasks.</span></span> <span data-ttu-id="97398-183">Därefter du skapa och köra hello-projektet.</span><span class="sxs-lookup"><span data-stu-id="97398-183">Next, you'll build and run hello project.</span></span>

## <a name="build-hello-project"></a><span data-ttu-id="97398-184">Skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="97398-184">Build hello project</span></span>
1. <span data-ttu-id="97398-185">Högerklicka på hello projektnamn i Projektutforskaren, och välj **kör som** > **Maven build...**  tooconfigure hello projektet.</span><span class="sxs-lookup"><span data-stu-id="97398-185">In Project Explorer, right-click hello project name and choose **Run As** > **Maven build...** tooconfigure hello project.</span></span>
   
    ![][10]
2. <span data-ttu-id="97398-186">I Goals i Edit Configuration skriver du ”clean install” och klickar på **Run**.</span><span class="sxs-lookup"><span data-stu-id="97398-186">In Edit Configuration, in Goals, type "clean install", and then click **Run**.</span></span>

<span data-ttu-id="97398-187">Statusmeddelanden är utdata toohello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="97398-187">Status messages are output toohello console window.</span></span> <span data-ttu-id="97398-188">Du bör se Skapa lyckade som anger hello-projektet skapats utan fel.</span><span class="sxs-lookup"><span data-stu-id="97398-188">You should see BUILD SUCCESS indicating hello project built without errors.</span></span>

## <a name="run-hello-app"></a><span data-ttu-id="97398-189">Kör hello-appen</span><span class="sxs-lookup"><span data-stu-id="97398-189">Run hello app</span></span>
<span data-ttu-id="97398-190">I det sista steget körs hello program i en lokal server-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="97398-190">In this last step, you will run hello application in a local server runtime environment.</span></span>

<span data-ttu-id="97398-191">Om du inte har angett en server körningsmiljö ännu i Eclipse, behöver du toodo som först.</span><span class="sxs-lookup"><span data-stu-id="97398-191">If you haven't yet specified a server runtime environment in Eclipse, you'll need toodo that first.</span></span>

1. <span data-ttu-id="97398-192">Expandera **WebContent** i Project Explorer.</span><span class="sxs-lookup"><span data-stu-id="97398-192">In Project Explorer, expand **WebContent**.</span></span>
2. <span data-ttu-id="97398-193">Högerklicka på **Search.jsp** > **Run As** > **Run on Server**.</span><span class="sxs-lookup"><span data-stu-id="97398-193">Right-click **Search.jsp** > **Run As** > **Run on Server**.</span></span> <span data-ttu-id="97398-194">Välj hello Apache Tomcat server och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="97398-194">Select hello Apache Tomcat server, and then click **Run**.</span></span>

> [!TIP]
> <span data-ttu-id="97398-195">Om du använder en icke-förvalt arbetsytan toostore projektet, behöver du toomodify **kör konfiguration** toopoint toohello projekt plats tooavoid ett serverfel uppstart.</span><span class="sxs-lookup"><span data-stu-id="97398-195">If you used a non-default workspace toostore your project, you'll need toomodify **Run Configuration** toopoint toohello project location tooavoid a server start-up error.</span></span> <span data-ttu-id="97398-196">I Project Explorer högerklickar du på **Search.jsp** > **Run As** > **Run Configurations**.</span><span class="sxs-lookup"><span data-stu-id="97398-196">In Project Explorer, right-click **Search.jsp** > **Run As** > **Run Configurations**.</span></span> <span data-ttu-id="97398-197">Välj hello Apache Tomcat-server.</span><span class="sxs-lookup"><span data-stu-id="97398-197">Select hello Apache Tomcat server.</span></span> <span data-ttu-id="97398-198">Klicka på **Arguments**.</span><span class="sxs-lookup"><span data-stu-id="97398-198">Click **Arguments**.</span></span> <span data-ttu-id="97398-199">Klicka på **arbetsytan** eller **filsystemet** tooset hello mapp som innehåller hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="97398-199">Click **Workspace** or **File System** tooset hello folder containing hello project.</span></span>
> 
> 

<span data-ttu-id="97398-200">När du kör programmet hello bör du se ett webbläsarfönster sökrutan för att ange villkoren.</span><span class="sxs-lookup"><span data-stu-id="97398-200">When you run hello application, you should see a browser window, providing a search box for entering terms.</span></span>

<span data-ttu-id="97398-201">Vänta ungefär en minut innan du klickar på **Sök** toogive hello service tid toocreate och Läs in hello index.</span><span class="sxs-lookup"><span data-stu-id="97398-201">Wait about one minute before clicking **Search** toogive hello service time toocreate and load hello index.</span></span> <span data-ttu-id="97398-202">Om du får en HTTP 404-fel, måste du bara toowait lite längre innan du försöker igen.</span><span class="sxs-lookup"><span data-stu-id="97398-202">If you get an HTTP 404 error, you just need toowait a little bit longer before trying again.</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="97398-203">Söka i USGS-data</span><span class="sxs-lookup"><span data-stu-id="97398-203">Search on USGS data</span></span>
<span data-ttu-id="97398-204">Hej USGS datauppsättningen innehåller poster som är relevanta toohello tillstånd Rhode dataö.</span><span class="sxs-lookup"><span data-stu-id="97398-204">hello USGS data set includes records that are relevant toohello state of Rhode Island.</span></span> <span data-ttu-id="97398-205">Om du klickar på **Sök** för en tom sökrutan visas hello upp 50 poster som är hello standard.</span><span class="sxs-lookup"><span data-stu-id="97398-205">If you click **Search** on an empty search box, you will get hello top 50 entries, which is hello default.</span></span>

<span data-ttu-id="97398-206">Ange en sökterm ger hello sökmotor något toogo på.</span><span class="sxs-lookup"><span data-stu-id="97398-206">Entering a search term will give hello search engine something toogo on.</span></span> <span data-ttu-id="97398-207">Prova att skriva namnet på någon från regionen.</span><span class="sxs-lookup"><span data-stu-id="97398-207">Try entering a regional name.</span></span> <span data-ttu-id="97398-208">”Roger Williams” var hello första resursstyrningen av Rhode dataö.</span><span class="sxs-lookup"><span data-stu-id="97398-208">"Roger Williams" was hello first governor of Rhode Island.</span></span> <span data-ttu-id="97398-209">Många parker, byggnader och skolor bär hans namn.</span><span class="sxs-lookup"><span data-stu-id="97398-209">Numerous parks, buildings, and schools are named after him.</span></span>

![][11]

<span data-ttu-id="97398-210">Du kan också prova någon av dessa söktermer:</span><span class="sxs-lookup"><span data-stu-id="97398-210">You could also try any of these terms:</span></span>

* <span data-ttu-id="97398-211">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="97398-211">Pawtucket</span></span>
* <span data-ttu-id="97398-212">Pembroke</span><span class="sxs-lookup"><span data-stu-id="97398-212">Pembroke</span></span>
* <span data-ttu-id="97398-213">goose +cape</span><span class="sxs-lookup"><span data-stu-id="97398-213">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="97398-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97398-214">Next steps</span></span>
<span data-ttu-id="97398-215">Detta är hello första Azure Search-självstudierna baserat på Java och hello USGS dataset.</span><span class="sxs-lookup"><span data-stu-id="97398-215">This is hello first Azure Search tutorial based on Java and hello USGS dataset.</span></span> <span data-ttu-id="97398-216">Över tiden, kommer vi utöka den här självstudiekursen toodemonstrate ytterligare sökfunktioner som du kanske vill toouse i din anpassade lösningar.</span><span class="sxs-lookup"><span data-stu-id="97398-216">Over time, we'll extend this tutorial toodemonstrate additional search features you might want toouse in your custom solutions.</span></span>

<span data-ttu-id="97398-217">Om du redan har vissa bakgrunden i Azure Search kan du kan använda det här exemplet som en springboard för ytterligare undersökningar kanske höja hello [söksidan](search-pagination-page-layout.md), eller implementera [fasetterad navigering](search-faceted-navigation.md).</span><span class="sxs-lookup"><span data-stu-id="97398-217">If you already have some background in Azure Search, you can use this sample as a springboard for further experimentation, perhaps augmenting hello [search page](search-pagination-page-layout.md), or implementing [faceted navigation](search-faceted-navigation.md).</span></span> <span data-ttu-id="97398-218">Du kan också förbättra vid hello sökresultatsidan genom att lägga till antal och batchbearbetning dokument så att användare kan bläddra igenom hello resultat.</span><span class="sxs-lookup"><span data-stu-id="97398-218">You can also improve upon hello search results page by adding counts and batching documents so that users can page through hello results.</span></span>

<span data-ttu-id="97398-219">Nya tooAzure sökningen?</span><span class="sxs-lookup"><span data-stu-id="97398-219">New tooAzure Search?</span></span> <span data-ttu-id="97398-220">Vi rekommenderar att du försöker andra självstudiekurser toodevelop förstå vad du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="97398-220">We recommend trying other tutorials toodevelop an understanding of what you can create.</span></span> <span data-ttu-id="97398-221">Besök vår [dokumentationssidan](https://azure.microsoft.com/documentation/services/search/) toofind mer resurser.</span><span class="sxs-lookup"><span data-stu-id="97398-221">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) toofind more resources.</span></span> <span data-ttu-id="97398-222">Du kan också visa hello länkar i vår [Video-och kursen](search-video-demo-tutorial-list.md) tooaccess mer information.</span><span class="sxs-lookup"><span data-stu-id="97398-222">You can also view hello links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) tooaccess more information.</span></span>

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
