---
title: "Komma igång med Azure Search i Java| Microsoft Docs"
description: "Här lär du dig hur du skapar ett värdbaserat sökprogram i molnet med Azure och Java som programmeringsspråk."
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
ms.openlocfilehash: f6ca06a0349def97b38a1bf6d0d8f36236077e92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-search-in-java"></a><span data-ttu-id="4a4f3-103">Komma igång med Azure Search i Java</span><span class="sxs-lookup"><span data-stu-id="4a4f3-103">Get started with Azure Search in Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4a4f3-104">Portal</span><span class="sxs-lookup"><span data-stu-id="4a4f3-104">Portal</span></span>](search-get-started-portal.md)
> * [<span data-ttu-id="4a4f3-105">NET</span><span class="sxs-lookup"><span data-stu-id="4a4f3-105">.NET</span></span>](search-howto-dotnet-sdk.md)
> 
> 

<span data-ttu-id="4a4f3-106">Lär dig hur du skapar ett anpassat Java-sökprogram som använder Azure Search som sökmiljö.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-106">Learn how to build a custom Java search application that uses Azure Search for its search experience.</span></span> <span data-ttu-id="4a4f3-107">I den här självstudiekursen används [REST-API:et för tjänsten Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) för att skapa de objekt och åtgärder som används i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-107">This tutorial uses the [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) to construct the objects and operations used in this exercise.</span></span>

<span data-ttu-id="4a4f3-108">Om du vill köra det här exemplet måste du ha en Azure Search-tjänst, som du kan registrera dig för på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-108">To run this sample, you must have an Azure Search service, which you can sign up for in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="4a4f3-109">Stegvisa instruktioner finns i [Skapa en Azure Search-tjänst på portalen](search-create-service-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-109">See [Create an Azure Search service in the portal](search-create-service-portal.md) for step-by-step instructions.</span></span>

<span data-ttu-id="4a4f3-110">Vi använde följande programvara när vi skapade och testade det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="4a4f3-110">We used the following software to build and test this sample:</span></span>

* <span data-ttu-id="4a4f3-111">[Eclipse IDE för Java EE-utvecklare](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-111">[Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar).</span></span> <span data-ttu-id="4a4f3-112">Var noga med att ladda ned EE-versionen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-112">Be sure to download the EE version.</span></span> <span data-ttu-id="4a4f3-113">Ett av verifieringsstegen kräver en funktion som bara finns i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-113">One of the verification steps requires a feature that is found only in this edition.</span></span>
* [<span data-ttu-id="4a4f3-114">JDK 8u40</span><span class="sxs-lookup"><span data-stu-id="4a4f3-114">JDK 8u40</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [<span data-ttu-id="4a4f3-115">Apache Tomcat 8.0</span><span class="sxs-lookup"><span data-stu-id="4a4f3-115">Apache Tomcat 8.0</span></span>](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a><span data-ttu-id="4a4f3-116">Om de data som används</span><span class="sxs-lookup"><span data-stu-id="4a4f3-116">About the data</span></span>
<span data-ttu-id="4a4f3-117">Det här exempelprogrammet använder data från [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), som har filtrerats på delstaten Rhode Island för att minska datauppsättningens storlek.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-117">This sample application uses data from the [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtered on the state of Rhode Island to reduce the dataset size.</span></span> <span data-ttu-id="4a4f3-118">Vi ska använda dessa data för att skapa ett sökprogram som returnerar viktiga byggnader som sjukhus och skolor, samt geologiska element som vattendrag, sjöar och bergstoppar.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-118">We'll use this data to build a search application that returns landmark buildings such as hospitals and schools, as well as geological features like streams, lakes, and summits.</span></span>

<span data-ttu-id="4a4f3-119">I det här programmet bygger och läser programmet **SearchServlet.java** in indexet med hjälp av en [indexeringskonstruktion](https://msdn.microsoft.com/library/azure/dn798918.aspx) och hämtar den filtrerade USGS-datauppsättningen från en offentlig Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-119">In this application, the **SearchServlet.java** program builds and loads the index using an [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) construct, retrieving the filtered USGS dataset from a public Azure SQL Database.</span></span> <span data-ttu-id="4a4f3-120">Fördefinierade autentiseringsuppgifter och anslutningsinformation för onlinedatakällan finns i programkoden.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-120">Predefined credentials and connection  information to the online data source are provided in the program code.</span></span> <span data-ttu-id="4a4f3-121">Ingen ytterligare konfiguration krävs vad gäller dataåtkomsten.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-121">In terms of data access, no further configuration is necessary.</span></span>

> [!NOTE]
> <span data-ttu-id="4a4f3-122">Vi har använt ett filter för den här datauppsättningen för att hålla oss under gränsen på 10 000 dokument för den kostnadsfria prisnivån.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-122">We applied a filter on this dataset to stay under the 10,000 document limit of the free pricing tier.</span></span> <span data-ttu-id="4a4f3-123">Om du använder standardnivån så gäller inte den här gränsen och du kan ändra koden om du vill använda en större datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-123">If you use the standard tier, this limit does not apply, and you can modify this code to use a bigger dataset.</span></span> <span data-ttu-id="4a4f3-124">Mer information om kapaciteten för varje prisnivå finns i [Gränser och begränsningar](search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-124">For details about capacity for each pricing tier, see [Limits and constraints](search-limits-quotas-capacity.md).</span></span>
> 
> 

## <a name="about-the-program-files"></a><span data-ttu-id="4a4f3-125">Om programfilerna</span><span class="sxs-lookup"><span data-stu-id="4a4f3-125">About the program files</span></span>
<span data-ttu-id="4a4f3-126">Följande lista beskriver de filer som är relevanta för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-126">The following list describes the files that are relevant to this sample.</span></span>

* <span data-ttu-id="4a4f3-127">Search.JSP: Tillhandahåller användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="4a4f3-127">Search.jsp: Provides the user interface</span></span>
* <span data-ttu-id="4a4f3-128">SearchServlet.java: Tillhandahåller metoder (påminner om en kontrollant i MVC)</span><span class="sxs-lookup"><span data-stu-id="4a4f3-128">SearchServlet.java: Provides methods (similar to a controller in MVC)</span></span>
* <span data-ttu-id="4a4f3-129">SearchServiceClient.java: Hanterar HTTP-begäranden</span><span class="sxs-lookup"><span data-stu-id="4a4f3-129">SearchServiceClient.java: Handles HTTP requests</span></span>
* <span data-ttu-id="4a4f3-130">SearchServiceHelper.java: En hjälparklass som tillhandahåller statiska metoder</span><span class="sxs-lookup"><span data-stu-id="4a4f3-130">SearchServiceHelper.java: A helper class that provides static methods</span></span>
* <span data-ttu-id="4a4f3-131">Document.Java: Tillhandahåller datamodellen</span><span class="sxs-lookup"><span data-stu-id="4a4f3-131">Document.java: Provides the data model</span></span>
* <span data-ttu-id="4a4f3-132">Config.Properties: Anger URL:en och API-nyckeln för Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="4a4f3-132">config.properties: Sets the Search service URL and api-key</span></span>
* <span data-ttu-id="4a4f3-133">Pom.XML: Ett Maven-beroende</span><span class="sxs-lookup"><span data-stu-id="4a4f3-133">Pom.xml: A Maven dependency</span></span>

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a><span data-ttu-id="4a4f3-134">Leta upp tjänstnamnet och API-nyckeln för Azure Search-tjänsten</span><span class="sxs-lookup"><span data-stu-id="4a4f3-134">Find the service name and api-key of your Azure Search service</span></span>
<span data-ttu-id="4a4f3-135">Alla REST API-anrop till Azure Search kräver att du anger tjänstens URL och en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-135">All REST API calls into Azure Search require that you provide the service URL and an api-key.</span></span> 

1. <span data-ttu-id="4a4f3-136">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-136">Sign in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4a4f3-137">I snabbåtkomstfältet klickar du på **Söktjänst** för att visa en lista över Azure Search-tjänsterna som har etablerats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-137">In the jump bar, click **Search service** to list all of the Azure Search services provisioned for your subscription.</span></span>
3. <span data-ttu-id="4a4f3-138">Markera den tjänst som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-138">Select the service you want to use.</span></span>
4. <span data-ttu-id="4a4f3-139">På instrumentpanelen för tjänsten ser du paneler för viktig information samt nyckelikonen för att komma åt administatörsnycklarna.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-139">On the service dashboard, you'll see tiles for essential information as well as the key icon for accessing the admin keys.</span></span>
   
      ![][3]
5. <span data-ttu-id="4a4f3-140">Kopiera tjänstens URL och en administratörsnyckel.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-140">Copy the service URL and an admin key.</span></span> <span data-ttu-id="4a4f3-141">Du behöver dem senare när du lägger till dem i filen **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-141">You will need them later, when you add them to the **config.properties** file.</span></span>

## <a name="download-the-sample-files"></a><span data-ttu-id="4a4f3-142">Ladda ned exempelfilerna</span><span class="sxs-lookup"><span data-stu-id="4a4f3-142">Download the sample files</span></span>
1. <span data-ttu-id="4a4f3-143">Gå till [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-143">Go to [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) on GitHub.</span></span>
2. <span data-ttu-id="4a4f3-144">Klicka på **Ladda ned ZIP**, spara ZIP-filen på disk och extrahera sedan alla filer som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-144">Click **Download ZIP**, save the .zip file to disk, and then extract all the files it contains.</span></span> <span data-ttu-id="4a4f3-145">Om du vill kan du extrahera filerna till Java-arbetsytan så att det blir lättare att hitta projektet senare.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-145">Consider extracting the files into your Java workspace to make it easier to find the project later.</span></span>
3. <span data-ttu-id="4a4f3-146">Exempelfilerna är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-146">The sample files are read-only.</span></span> <span data-ttu-id="4a4f3-147">Högerklicka på Mappegenskaper och ta bort skrivskyddet.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-147">Right-click folder properties and clear the read-only attribute.</span></span>

<span data-ttu-id="4a4f3-148">Alla efterföljande filändringar och körningsinstruktioner görs mot filer i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-148">All subsequent file modifications and run statements will be made against files in this folder.</span></span>  

## <a name="import-project"></a><span data-ttu-id="4a4f3-149">Importera projekt</span><span class="sxs-lookup"><span data-stu-id="4a4f3-149">Import project</span></span>
1. <span data-ttu-id="4a4f3-150">I Eclipse väljer du **File** > **Import** > **General** > **Existing Projects into Workspace**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-150">In Eclipse, choose **File** > **Import** > **General** > **Existing Projects into Workspace**.</span></span>
   
    ![][4]
2. <span data-ttu-id="4a4f3-151">I **Select root directory** bläddrar du till mappen som innehåller exempelfilerna.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-151">In **Select root directory**, browse to the folder containing sample files.</span></span> <span data-ttu-id="4a4f3-152">Välj mappen som innehåller mappen .project.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-152">Select the folder that contains the .project folder.</span></span> <span data-ttu-id="4a4f3-153">Projektet bör visas i listan **Projects** som ett markerat objekt.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-153">The project should appear in the **Projects** list as a selected item.</span></span>
   
    ![][12]
3. <span data-ttu-id="4a4f3-154">Klicka på **Finish**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-154">Click **Finish**.</span></span>
4. <span data-ttu-id="4a4f3-155">Använd **Project Explorer** för att visa och redigera filerna.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-155">Use **Project Explorer** to view and edit the files.</span></span> <span data-ttu-id="4a4f3-156">Om den inte redan är öppen klickar du på **Window** > **Show view** > **Project Explorer** eller använder genvägen för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-156">If it's not already open, click **Window** > **Show View** > **Project Explorer** or use the shortcut to open it.</span></span>

## <a name="configure-the-service-url-and-api-key"></a><span data-ttu-id="4a4f3-157">Konfigurera tjänstens URL och API-nyckel</span><span class="sxs-lookup"><span data-stu-id="4a4f3-157">Configure the service URL and api-key</span></span>
1. <span data-ttu-id="4a4f3-158">I **Project Explorer** dubbelklickar du på **config.properties** för att redigera konfigurationsinställningarna som innehåller servernamnet och API-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-158">In **Project Explorer**, double-click **config.properties** to edit the configuration settings containing the server name and api-key.</span></span>
2. <span data-ttu-id="4a4f3-159">Följ stegen ovan i den här artikeln, där du letade upp tjänstens URL och API-nyckeln på [Azure Portal](https://portal.azure.com), för att hämta de värden som du nu ska ange i **config.properties**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-159">Refer to the steps earlier in this article, where you found the service URL and api-key in the [Azure Portal](https://portal.azure.com), to get the values you will now enter into **config.properties**.</span></span>
3. <span data-ttu-id="4a4f3-160">I **config.properties** ersätter du ”Api Key” med API-nyckeln för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-160">In **config.properties**, replace "Api Key" with the api-key for your service.</span></span> <span data-ttu-id="4a4f3-161">Därefter ska tjänstnamnet (den första delen av URL:en http://servicename.search.windows.net) ersätta ”service name” i samma fil.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-161">Next, service name (the first component of the URL http://servicename.search.windows.net) replaces "service name" in the same file.</span></span>
   
    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a><span data-ttu-id="4a4f3-162">Konfigurera projektet, versionen och runtime-miljöerna</span><span class="sxs-lookup"><span data-stu-id="4a4f3-162">Configure the project, build and runtime environments</span></span>
1. <span data-ttu-id="4a4f3-163">I Eclipse högerklickar du på projektet i Project Explorer > **Properties** > **Project Facets**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-163">In Eclipse, in Project Explorer, right-click the project > **Properties** > **Project Facets**.</span></span>
2. <span data-ttu-id="4a4f3-164">Välj **Dynamic Web Module**, **Java** och **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-164">Select **Dynamic Web Module**, **Java**, and **JavaScript**.</span></span>
   
    ![][6]
3. <span data-ttu-id="4a4f3-165">Klicka på **Apply**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-165">Click **Apply**.</span></span>
4. <span data-ttu-id="4a4f3-166">Välj **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-166">Select **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add..**.</span></span>
5. <span data-ttu-id="4a4f3-167">Expandera Apache och välj den version av Apache Tomcat-servern som du installerade tidigare.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-167">Expand Apache and select the version of the Apache Tomcat server you previously installed.</span></span> <span data-ttu-id="4a4f3-168">I vårt system installerade vi version 8.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-168">On our system, we installed version 8.</span></span>
   
    ![][7]
6. <span data-ttu-id="4a4f3-169">Ange installationskatalogen för Tomcat på nästa sida.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-169">On the next page, specify the Tomcat installation directory.</span></span> <span data-ttu-id="4a4f3-170">På en Windows-dator är detta antagligen C:\Program\Apache Software Foundation\Tomcat *version*.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-170">On a Windows computer, this will most likely be C:\Program Files\Apache Software Foundation\Tomcat *version*.</span></span>
7. <span data-ttu-id="4a4f3-171">Klicka på **Finish**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-171">Click **Finish**.</span></span>
8. <span data-ttu-id="4a4f3-172">Välj **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-172">Select **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.</span></span>
9. <span data-ttu-id="4a4f3-173">Välj **Standard VM**i **Add JRE**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-173">In **Add JRE**, select **Standard VM**.</span></span>
10. <span data-ttu-id="4a4f3-174">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-174">Click **Next**.</span></span>
11. <span data-ttu-id="4a4f3-175">Klicka på **Directory** i JRE Definition på JRE-startsidan.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-175">In JRE Definition, in JRE home, click **Directory**.</span></span>
12. <span data-ttu-id="4a4f3-176">Gå till **Program Files** > **Java** och välj den JDK som du installerade tidigare.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-176">Navigate to **Program Files** > **Java** and select the JDK you previously installed.</span></span> <span data-ttu-id="4a4f3-177">Det är viktigt att du väljer JDK som JRE.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-177">It's important to select the JDK as the JRE.</span></span>
13. <span data-ttu-id="4a4f3-178">Välj **JDK** i Installed JREs.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-178">In Installed JREs, choose the **JDK**.</span></span> <span data-ttu-id="4a4f3-179">Inställningarna bör se ut som i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-179">Your settings should look similar to the following screenshot.</span></span>
    
    ![][9]
14. <span data-ttu-id="4a4f3-180">Du kan också välja **Window** > **Web Browser** > **Internet Explorer** om du vill öppna programmet i ett externt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-180">Optionally, select **Window** > **Web Browser** > **Internet Explorer** to open the application in an external browser window.</span></span> <span data-ttu-id="4a4f3-181">En extern webbläsare ger dig en bättre webbupplevelse.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-181">Using an external browser gives you a better Web application experience.</span></span>
    
    ![][8]

<span data-ttu-id="4a4f3-182">Nu har du slutfört konfigurationsåtgärderna.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-182">You have now completed the configuration tasks.</span></span> <span data-ttu-id="4a4f3-183">Nu är det dags att bygga och köra projektet.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-183">Next, you'll build and run the project.</span></span>

## <a name="build-the-project"></a><span data-ttu-id="4a4f3-184">Bygga projektet</span><span class="sxs-lookup"><span data-stu-id="4a4f3-184">Build the project</span></span>
1. <span data-ttu-id="4a4f3-185">Högerklicka på projektets namn i Project Explorer och välj **Run as** > **Maven build** för att konfigurera projektet.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-185">In Project Explorer, right-click the project name and choose **Run As** > **Maven build...** to configure the project.</span></span>
   
    ![][10]
2. <span data-ttu-id="4a4f3-186">I Goals i Edit Configuration skriver du ”clean install” och klickar på **Run**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-186">In Edit Configuration, in Goals, type "clean install", and then click **Run**.</span></span>

<span data-ttu-id="4a4f3-187">Statusmeddelanden visas i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-187">Status messages are output to the console window.</span></span> <span data-ttu-id="4a4f3-188">Meddelandet BUILD SUCCESS bör visas som anger att projektet har skapats utan fel.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-188">You should see BUILD SUCCESS indicating the project built without errors.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4a4f3-189">Kör appen</span><span class="sxs-lookup"><span data-stu-id="4a4f3-189">Run the app</span></span>
<span data-ttu-id="4a4f3-190">I det sista steget ska du köra programmet i körningsmiljön för en lokal server.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-190">In this last step, you will run the application in a local server runtime environment.</span></span>

<span data-ttu-id="4a4f3-191">Om du inte har angett serverkörningsmiljön i Eclipse än så måste du göra det först.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-191">If you haven't yet specified a server runtime environment in Eclipse, you'll need to do that first.</span></span>

1. <span data-ttu-id="4a4f3-192">Expandera **WebContent** i Project Explorer.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-192">In Project Explorer, expand **WebContent**.</span></span>
2. <span data-ttu-id="4a4f3-193">Högerklicka på **Search.jsp** > **Run As** > **Run on Server**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-193">Right-click **Search.jsp** > **Run As** > **Run on Server**.</span></span> <span data-ttu-id="4a4f3-194">Välj Apache Tomcat-servern och klicka på **Run**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-194">Select the Apache Tomcat server, and then click **Run**.</span></span>

> [!TIP]
> <span data-ttu-id="4a4f3-195">Om du använder en annan arbetsyta än en standardarbetsyta för att lagra projektet måste du ändra **Run Configuration** så att det pekar på projektets plats för att undvika fel när servern startar.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-195">If you used a non-default workspace to store your project, you'll need to modify **Run Configuration** to point to the project location to avoid a server start-up error.</span></span> <span data-ttu-id="4a4f3-196">I Project Explorer högerklickar du på **Search.jsp** > **Run As** > **Run Configurations**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-196">In Project Explorer, right-click **Search.jsp** > **Run As** > **Run Configurations**.</span></span> <span data-ttu-id="4a4f3-197">Välj Apache Tomcat-servern.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-197">Select the Apache Tomcat server.</span></span> <span data-ttu-id="4a4f3-198">Klicka på **Arguments**.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-198">Click **Arguments**.</span></span> <span data-ttu-id="4a4f3-199">Klicka på **Workspace** eller **File System** för att ange mappen som innehåller projektet.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-199">Click **Workspace** or **File System** to set the folder containing the project.</span></span>
> 
> 

<span data-ttu-id="4a4f3-200">När du kör programmet bör du se ett webbläsarfönster med en sökruta där du kan ange söktermer.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-200">When you run the application, you should see a browser window, providing a search box for entering terms.</span></span>

<span data-ttu-id="4a4f3-201">Vänta ungefär en minut innan du klickar på **Search** så att tjänsten får tid på sig att skapa och läsa in indexet.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-201">Wait about one minute before clicking **Search** to give the service time to create and load the index.</span></span> <span data-ttu-id="4a4f3-202">Om ett HTTP 404-fel returneras väntar du bara lite längre innan du försöker igen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-202">If you get an HTTP 404 error, you just need to wait a little bit longer before trying again.</span></span>

## <a name="search-on-usgs-data"></a><span data-ttu-id="4a4f3-203">Söka i USGS-data</span><span class="sxs-lookup"><span data-stu-id="4a4f3-203">Search on USGS data</span></span>
<span data-ttu-id="4a4f3-204">USGS-datauppsättningen innehåller poster som är relevanta för delstaten Rhode Island.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-204">The USGS data set includes records that are relevant to the state of Rhode Island.</span></span> <span data-ttu-id="4a4f3-205">Om du klickar på **Search** i en tom sökrutan returneras 50 poster, vilket är standard.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-205">If you click **Search** on an empty search box, you will get the top 50 entries, which is the default.</span></span>

<span data-ttu-id="4a4f3-206">Om du skriver en sökterm ger du sökmotorn något att gå på.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-206">Entering a search term will give the search engine something to go on.</span></span> <span data-ttu-id="4a4f3-207">Prova att skriva namnet på någon från regionen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-207">Try entering a regional name.</span></span> <span data-ttu-id="4a4f3-208">”Roger Williams” var Rhode Islands första guvernör.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-208">"Roger Williams" was the first governor of Rhode Island.</span></span> <span data-ttu-id="4a4f3-209">Många parker, byggnader och skolor bär hans namn.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-209">Numerous parks, buildings, and schools are named after him.</span></span>

![][11]

<span data-ttu-id="4a4f3-210">Du kan också prova någon av dessa söktermer:</span><span class="sxs-lookup"><span data-stu-id="4a4f3-210">You could also try any of these terms:</span></span>

* <span data-ttu-id="4a4f3-211">Pawtucket</span><span class="sxs-lookup"><span data-stu-id="4a4f3-211">Pawtucket</span></span>
* <span data-ttu-id="4a4f3-212">Pembroke</span><span class="sxs-lookup"><span data-stu-id="4a4f3-212">Pembroke</span></span>
* <span data-ttu-id="4a4f3-213">goose +cape</span><span class="sxs-lookup"><span data-stu-id="4a4f3-213">goose +cape</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a4f3-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a4f3-214">Next steps</span></span>
<span data-ttu-id="4a4f3-215">Det här är den första Azure Search-självstudiekursen som baseras på Java och USGS-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-215">This is the first Azure Search tutorial based on Java and the USGS dataset.</span></span> <span data-ttu-id="4a4f3-216">Med tiden kommer vi att utöka den här självstudiekursen och demonstrera ytterligare sökfunktioner som du kanske vill använda i dina anpassade lösningar.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-216">Over time, we'll extend this tutorial to demonstrate additional search features you might want to use in your custom solutions.</span></span>

<span data-ttu-id="4a4f3-217">Om du redan har viss erfarenhet av Azure Search kan du använda det här exemplet som en utgångspunkt för ytterligare experiment och kanske utöka [söksidan](search-pagination-page-layout.md) eller implementera [aspektbaserad navigering](search-faceted-navigation.md).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-217">If you already have some background in Azure Search, you can use this sample as a springboard for further experimentation, perhaps augmenting the [search page](search-pagination-page-layout.md), or implementing [faceted navigation](search-faceted-navigation.md).</span></span> <span data-ttu-id="4a4f3-218">Du kan även förbättra sidan med sökresultat genom att lägga till antal och batchbearbeta dokument så att användarna kan bläddra igenom resultaten.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-218">You can also improve upon the search results page by adding counts and batching documents so that users can page through the results.</span></span>

<span data-ttu-id="4a4f3-219">Har du inte provat Azure Search än?</span><span class="sxs-lookup"><span data-stu-id="4a4f3-219">New to Azure Search?</span></span> <span data-ttu-id="4a4f3-220">Vi rekommenderar att du går andra självstudiekurser så att du ser vad du kan skapa.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-220">We recommend trying other tutorials to develop an understanding of what you can create.</span></span> <span data-ttu-id="4a4f3-221">Vår [dokumentationssida](https://azure.microsoft.com/documentation/services/search/) innehåller fler resurser.</span><span class="sxs-lookup"><span data-stu-id="4a4f3-221">Visit our [documentation page](https://azure.microsoft.com/documentation/services/search/) to find more resources.</span></span> <span data-ttu-id="4a4f3-222">Mer information finns också på länkarna i [listan med videoklipp och självstudiekurser](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="4a4f3-222">You can also view the links in our [Video and Tutorial list](search-video-demo-tutorial-list.md) to access more information.</span></span>

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
