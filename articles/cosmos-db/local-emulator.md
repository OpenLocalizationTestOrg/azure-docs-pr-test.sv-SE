---
title: aaaDevelop lokalt med hello Azure Cosmos DB emulatorn | Microsoft Docs
description: "Med hello Azure Cosmos DB-emulatorn kan du utveckla och testa programmet lokalt för gratis, utan att skapa en Azure-prenumeration."
services: cosmos-db
documentationcenter: 
keywords: Azure Cosmos DB-emulatorn
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: arramac
ms.openlocfilehash: fb5449489e5f71664e72d8e11e583315be371bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cosmos-db-emulator-for-local-development-and-testing"></a><span data-ttu-id="c9d51-104">Använd hello Azure Cosmos DB-emulatorn för lokal utveckling och testning</span><span class="sxs-lookup"><span data-stu-id="c9d51-104">Use hello Azure Cosmos DB Emulator for local development and testing</span></span>

<table>
<tr>
  <td><span data-ttu-id="c9d51-105"><strong>Binärfiler</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-105"><strong>Binaries</strong></span></span></td>
  <td>[<span data-ttu-id="c9d51-106">Ladda ned MSI</span><span class="sxs-lookup"><span data-stu-id="c9d51-106">Download MSI</span></span>](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-107"><strong>Docker</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-107"><strong>Docker</strong></span></span></td>
  <td>[<span data-ttu-id="c9d51-108">Docker-hubb</span><span class="sxs-lookup"><span data-stu-id="c9d51-108">Docker Hub</span></span>](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-109"><strong>Docker-källa</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-109"><strong>Docker source</strong></span></span></td>
  <td>[<span data-ttu-id="c9d51-110">Github</span><span class="sxs-lookup"><span data-stu-id="c9d51-110">Github</span></span>](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
<span data-ttu-id="c9d51-111">hello Azure Cosmos DB emulatorn tillhandahåller en lokal miljö som emulerar hello Azure DB som Cosmos-tjänsten för utveckling.</span><span class="sxs-lookup"><span data-stu-id="c9d51-111">hello Azure Cosmos DB Emulator provides a local environment that emulates hello Azure Cosmos DB service for development purposes.</span></span> <span data-ttu-id="c9d51-112">Använda hello Azure Cosmos DB-emulatorn kan du utveckla och testa programmet lokalt, utan att skapa en Azure-prenumeration eller kostnader.</span><span class="sxs-lookup"><span data-stu-id="c9d51-112">Using hello Azure Cosmos DB Emulator, you can develop and test your application locally, without creating an Azure subscription or incurring any costs.</span></span> <span data-ttu-id="c9d51-113">När du är nöjd med hur programmet fungerar i hello Azure Cosmos DB-emulatorn kan växla du toousing ett Azure DB som Cosmos-konto i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c9d51-113">When you're satisfied with how your application is working in hello Azure Cosmos DB Emulator, you can switch toousing an Azure Cosmos DB account in hello cloud.</span></span>

<span data-ttu-id="c9d51-114">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c9d51-114">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="c9d51-115">Installera hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-115">Installing hello Emulator</span></span>
> * <span data-ttu-id="c9d51-116">Med Docker för Windows hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-116">Running hello Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="c9d51-117">Autentisering av förfrågningar</span><span class="sxs-lookup"><span data-stu-id="c9d51-117">Authenticating requests</span></span>
> * <span data-ttu-id="c9d51-118">Med hjälp av hello Data Explorer i hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-118">Using hello Data Explorer in hello Emulator</span></span>
> * <span data-ttu-id="c9d51-119">Exportera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="c9d51-119">Exporting SSL certificates</span></span>
> * <span data-ttu-id="c9d51-120">Anropar hello Emulator från hello kommandorad</span><span class="sxs-lookup"><span data-stu-id="c9d51-120">Calling hello Emulator from hello command line</span></span>
> * <span data-ttu-id="c9d51-121">Samla in spårningsfiler</span><span class="sxs-lookup"><span data-stu-id="c9d51-121">Collecting trace files</span></span>

<span data-ttu-id="c9d51-122">Vi rekommenderar att komma igång genom att titta på hello följande video, där Kirill Gavrylyuk visar hur tooget igång med hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-122">We recommend getting started by watching hello following video, where Kirill Gavrylyuk shows how tooget started with hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="c9d51-123">Observera att hello video refererar toohello emulatorn som hello DocumentDB-emulatorn, men själva hello-verktyget har bytt hello Azure Cosmos DB emulatorn sedan in hello video.</span><span class="sxs-lookup"><span data-stu-id="c9d51-123">Note that hello video refers toohello emulator as hello DocumentDB Emulator, but hello tool itself has been renamed hello Azure Cosmos DB Emulator since taping hello video.</span></span> <span data-ttu-id="c9d51-124">All information i hello video är korrekt för hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-124">All information in hello video is still accurate for hello Azure Cosmos DB Emulator.</span></span> 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-hello-emulator-works"></a><span data-ttu-id="c9d51-125">Så här fungerar hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-125">How hello Emulator works</span></span>
<span data-ttu-id="c9d51-126">hello Azure Cosmos DB emulatorn ger en hög återgivning emulering av hello Azure DB som Cosmos-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c9d51-126">hello Azure Cosmos DB Emulator provides a high-fidelity emulation of hello Azure Cosmos DB service.</span></span> <span data-ttu-id="c9d51-127">Den stöder identiska funktioner som Azure Cosmos DB, inklusive stöd för att skapa och hämtning av JSON-dokument, etablering och skalning samlingar och köra lagrade procedurer och utlösare.</span><span class="sxs-lookup"><span data-stu-id="c9d51-127">It supports identical functionality as Azure Cosmos DB, including support for creating and querying JSON documents, provisioning and scaling collections, and executing stored procedures and triggers.</span></span> <span data-ttu-id="c9d51-128">Du kan utveckla och testa program med hjälp av hello Azure Cosmos DB-emulatorn och distribuera dem tooAzure på global nivå genom att bara göra en enda konfigurationsändring toohello Anslutningens slutpunkt för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c9d51-128">You can develop and test applications using hello Azure Cosmos DB Emulator, and deploy them tooAzure at global scale by just making a single configuration change toohello connection endpoint for Azure Cosmos DB.</span></span>

<span data-ttu-id="c9d51-129">När vi har skapat en lokal emulering hög återgivning av hello faktiska Azure Cosmos DB service är hello implementering av hello Azure Cosmos DB-emulatorn än hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c9d51-129">While we created a high-fidelity local emulation of hello actual Azure Cosmos DB service, hello implementation of hello Azure Cosmos DB Emulator is different than that of hello service.</span></span> <span data-ttu-id="c9d51-130">Till exempel använder hello Azure Cosmos DB emulatorn standard OS-komponenter, till exempel hello lokalt filsystem för beständighet och HTTPS-protokoll-stacken för anslutning.</span><span class="sxs-lookup"><span data-stu-id="c9d51-130">For example, hello Azure Cosmos DB Emulator uses standard OS components such as hello local file system for persistence, and HTTPS protocol stack for connectivity.</span></span> <span data-ttu-id="c9d51-131">Detta innebär att vissa funktioner som förlitar sig på Azure-infrastrukturen som globala replikering, en siffra millisekunds fördröjning för läsning/skrivning och justerbara konsekvensnivåer inte är tillgängliga via hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-131">This means that some functionality that relies on Azure infrastructure like global replication, single-digit millisecond latency for reads/writes, and tunable consistency levels are not available via hello Azure Cosmos DB Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="c9d51-132">På den här gången hello Data Explorer i hello stöder emulatorn endast hello skapandet av DocumentDB API samlingar och MongoDB-samlingar.</span><span class="sxs-lookup"><span data-stu-id="c9d51-132">At this time hello Data Explorer in hello emulator only supports hello creation of DocumentDB API collections and MongoDB collections.</span></span> <span data-ttu-id="c9d51-133">hello Data Explorer i hello-emulatorn stöder för närvarande inte hello skapa tabeller och diagram.</span><span class="sxs-lookup"><span data-stu-id="c9d51-133">hello Data Explorer in hello emulator does not currently support hello creation of tables and graphs.</span></span> 

## <a name="system-requirements"></a><span data-ttu-id="c9d51-134">Systemkrav</span><span class="sxs-lookup"><span data-stu-id="c9d51-134">System requirements</span></span>
<span data-ttu-id="c9d51-135">hello Azure Cosmos DB-emulatorn har hello följande maskin- och programvarukrav:</span><span class="sxs-lookup"><span data-stu-id="c9d51-135">hello Azure Cosmos DB Emulator has hello following hardware and software requirements:</span></span>

* <span data-ttu-id="c9d51-136">Programvarukrav</span><span class="sxs-lookup"><span data-stu-id="c9d51-136">Software requirements</span></span>
  * <span data-ttu-id="c9d51-137">Windows Server 2012 R2, Windows Server 2016 eller Windows 10</span><span class="sxs-lookup"><span data-stu-id="c9d51-137">Windows Server 2012 R2, Windows Server 2016, or Windows 10</span></span>
*   <span data-ttu-id="c9d51-138">Minsta maskinvarukrav</span><span class="sxs-lookup"><span data-stu-id="c9d51-138">Minimum Hardware requirements</span></span>
  * <span data-ttu-id="c9d51-139">2 GB RAM-MINNE</span><span class="sxs-lookup"><span data-stu-id="c9d51-139">2 GB RAM</span></span>
  * <span data-ttu-id="c9d51-140">10 GB ledigt hårddiskutrymme</span><span class="sxs-lookup"><span data-stu-id="c9d51-140">10 GB available hard disk space</span></span>

## <a name="installation"></a><span data-ttu-id="c9d51-141">Installation</span><span class="sxs-lookup"><span data-stu-id="c9d51-141">Installation</span></span>
<span data-ttu-id="c9d51-142">Du kan hämta och installera hello Azure Cosmos DB emulatorn från hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="c9d51-142">You can download and install hello Azure Cosmos DB Emulator from hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span></span> 

> [!NOTE]
> <span data-ttu-id="c9d51-143">tooinstall, konfigurera och köra hello Azure Cosmos DB-emulatorn måste du ha administratörsbehörighet på hello-dator.</span><span class="sxs-lookup"><span data-stu-id="c9d51-143">tooinstall, configure, and run hello Azure Cosmos DB Emulator, you must have administrative privileges on hello computer.</span></span>

## <a name="running-on-docker-for-windows"></a><span data-ttu-id="c9d51-144">Körs på Docker för Windows</span><span class="sxs-lookup"><span data-stu-id="c9d51-144">Running on Docker for Windows</span></span>

<span data-ttu-id="c9d51-145">hello Azure Cosmos DB-emulatorn kan köras på Docker för Windows.</span><span class="sxs-lookup"><span data-stu-id="c9d51-145">hello Azure Cosmos DB Emulator can be run on Docker for Windows.</span></span> <span data-ttu-id="c9d51-146">hello emulatorn fungerar inte på Docker för Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="c9d51-146">hello Emulator does not work on Docker for Oracle Linux.</span></span>

<span data-ttu-id="c9d51-147">När du har [Docker för Windows](https://www.docker.com/docker-windows) installerat kan du dra hello emulatorn avbildningen från Docker-hubben genom att köra följande kommando från dina Favoriter shell hello (cmd.exe, PowerShell, etc.).</span><span class="sxs-lookup"><span data-stu-id="c9d51-147">Once you have [Docker for Windows](https://www.docker.com/docker-windows) installed, you can pull hello Emulator image from Docker Hub by running hello following command from your favorite shell (cmd.exe, PowerShell, etc.).</span></span>

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
<span data-ttu-id="c9d51-148">toostart hello avbildning, kör följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="c9d51-148">toostart hello image, run hello following commands.</span></span>

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

<span data-ttu-id="c9d51-149">hello svar ser ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="c9d51-149">hello response looks similar toohello following:</span></span>

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import hello SSL certificate from an administrator command prompt on hello host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

<span data-ttu-id="c9d51-150">Stänger hello interaktiva shell när hello emulatorn har startats kommer avstängning hello emulatorn behållare.</span><span class="sxs-lookup"><span data-stu-id="c9d51-150">Closing hello interactive shell once hello Emulator has been started will shutdown hello Emulator’s container.</span></span>

<span data-ttu-id="c9d51-151">Använd hello slutpunkt och huvudnyckeln i från hello svar i din klient och importera hello SSL-certifikatet till värden.</span><span class="sxs-lookup"><span data-stu-id="c9d51-151">Use hello endpoint and master key in from hello response in your client and import hello SSL certificate into your host.</span></span> <span data-ttu-id="c9d51-152">tooimport hello SSL-certifikat, hello följande från en kommandotolk för administratörer:</span><span class="sxs-lookup"><span data-stu-id="c9d51-152">tooimport hello SSL certificate, do hello following from an admin command prompt:</span></span>

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-hello-emulator"></a><span data-ttu-id="c9d51-153">Starta hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-153">Start hello Emulator</span></span>

<span data-ttu-id="c9d51-154">toostart hello Azure Cosmos DB-emulatorn, Välj hello Start-knappen eller tryck hello Windows-tangenten.</span><span class="sxs-lookup"><span data-stu-id="c9d51-154">toostart hello Azure Cosmos DB Emulator, select hello Start button or press hello Windows key.</span></span> <span data-ttu-id="c9d51-155">Börja skriva **Azure Cosmos DB emulatorn**, och välj hello-emulatorn hello listan med program.</span><span class="sxs-lookup"><span data-stu-id="c9d51-155">Begin typing **Azure Cosmos DB Emulator**, and select hello emulator from hello list of applications.</span></span> 

![Välj hello Start-knappen eller tryck på hello Windows-tangenten, börja skriva ** Azure Cosmos DB emulatorn ** och välj hello-emulatorn hello listan över program](./media/local-emulator/database-local-emulator-start.png)

<span data-ttu-id="c9d51-157">När emulatorn hello körs visas en ikon i hello meddelandefältet i Aktivitetsfältet.</span><span class="sxs-lookup"><span data-stu-id="c9d51-157">When hello emulator is running, you'll see an icon in hello Windows taskbar notification area.</span></span> ![Azure DB Cosmos lokala emulatorn Aktivitetsfältsmeddelande](./media/local-emulator/database-local-emulator-taskbar.png)

<span data-ttu-id="c9d51-159">hello Azure Cosmos DB emulatorn som standard körs på hello lokal dator (”localhost”) lyssnar på port 8081.</span><span class="sxs-lookup"><span data-stu-id="c9d51-159">hello Azure Cosmos DB Emulator by default runs on hello local machine ("localhost") listening on port 8081.</span></span>

<span data-ttu-id="c9d51-160">hello Azure Cosmos DB emulatorn installeras som standard toohello `C:\Program Files\Azure Cosmos DB Emulator` directory.</span><span class="sxs-lookup"><span data-stu-id="c9d51-160">hello Azure Cosmos DB Emulator is installed by default toohello `C:\Program Files\Azure Cosmos DB Emulator` directory.</span></span> <span data-ttu-id="c9d51-161">Du kan också starta och stoppa hello emulator från hello kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="c9d51-161">You can also start and stop hello emulator from hello command-line.</span></span> <span data-ttu-id="c9d51-162">Se [kommandoradsverktyget referens](#command-line) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c9d51-162">See [command-line tool reference](#command-line) for more information.</span></span>

## <a name="start-data-explorer"></a><span data-ttu-id="c9d51-163">Starta Data Explorer</span><span class="sxs-lookup"><span data-stu-id="c9d51-163">Start Data Explorer</span></span>

<span data-ttu-id="c9d51-164">När hello Azure Cosmos DB emulatorn startar öppnas automatiskt hello Azure Cosmos DB Data Explorer i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c9d51-164">When hello Azure Cosmos DB emulator launches it will automatically open hello Azure Cosmos DB Data Explorer in your browser.</span></span> <span data-ttu-id="c9d51-165">hello adress visas som [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span><span class="sxs-lookup"><span data-stu-id="c9d51-165">hello address will appear as [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span></span> <span data-ttu-id="c9d51-166">Om du stänger hello explorer och vill toore öppna det senare, du kan öppna hello URL i webbläsaren eller starta från hello Azure Cosmos DB emulatorn i hello Windows-ikon som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="c9d51-166">If you close hello explorer and would like toore-open it later, you can either open hello URL in your browser or launch it from hello Azure Cosmos DB Emulator in hello Windows Tray Icon as shown below.</span></span>

![Azure DB Cosmos lokala emulatorn data explorer programstart](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a><span data-ttu-id="c9d51-168">Söker efter uppdateringar</span><span class="sxs-lookup"><span data-stu-id="c9d51-168">Checking for updates</span></span>
<span data-ttu-id="c9d51-169">Data Explorer anger om det finns en ny uppdatering tillgänglig för hämtning.</span><span class="sxs-lookup"><span data-stu-id="c9d51-169">Data Explorer indicates if there is a new update available for download.</span></span> 

> [!NOTE]
> <span data-ttu-id="c9d51-170">Data som har skapats i en version av hello Azure Cosmos DB-emulatorn är inte garanterat toobe tillgänglig när du använder en annan version.</span><span class="sxs-lookup"><span data-stu-id="c9d51-170">Data created in one version of hello Azure Cosmos DB Emulator is not guaranteed toobe accessible when using a different version.</span></span> <span data-ttu-id="c9d51-171">Om du behöver toopersist data för hello lång sikt, rekommenderas att du lagrar data i ett Azure DB som Cosmos-konto i stället för hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-171">If you need toopersist your data for hello long term, it is recommended that you store that data in an Azure Cosmos DB account, rather than in hello Azure Cosmos DB Emulator.</span></span> 

## <a name="authenticating-requests"></a><span data-ttu-id="c9d51-172">Autentisering av förfrågningar</span><span class="sxs-lookup"><span data-stu-id="c9d51-172">Authenticating requests</span></span>
<span data-ttu-id="c9d51-173">Precis som med Azure Cosmos-DB i hello molnet måste varje begäran som du gör mot hello Azure Cosmos DB-emulatorn autentiseras.</span><span class="sxs-lookup"><span data-stu-id="c9d51-173">Just as with Azure Cosmos DB in hello cloud, every request that you make against hello Azure Cosmos DB Emulator must be authenticated.</span></span> <span data-ttu-id="c9d51-174">hello Azure Cosmos DB emulatorn stöder ett fast konto och en välkänd autentiseringsnyckel för huvudnyckeln för autentisering.</span><span class="sxs-lookup"><span data-stu-id="c9d51-174">hello Azure Cosmos DB Emulator supports a single fixed account and a well-known authentication key for master key authentication.</span></span> <span data-ttu-id="c9d51-175">Det här kontot och nyckeln är hello endast autentiseringsuppgifter som tillåts för användning med hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-175">This account and key are hello only credentials permitted for use with hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="c9d51-176">De är:</span><span class="sxs-lookup"><span data-stu-id="c9d51-176">They are:</span></span>

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> <span data-ttu-id="c9d51-177">hello huvudnyckel som stöds av hello Azure Cosmos DB-emulatorn är avsedd att användas endast med hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-177">hello master key supported by hello Azure Cosmos DB Emulator is intended for use only with hello emulator.</span></span> <span data-ttu-id="c9d51-178">Du kan inte använda din produktion Azure Cosmos DB konto och nyckel med hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-178">You cannot use your production Azure Cosmos DB account and key with hello Azure Cosmos DB Emulator.</span></span> 

> [!NOTE] 
> <span data-ttu-id="c9d51-179">Om du har startat hello emulator med hello/Key-alternativet kan sedan använda hello genereras nyckel i stället för ”C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw ==”</span><span class="sxs-lookup"><span data-stu-id="c9d51-179">If you have started hello emulator with hello /Key option, then use hello generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="c9d51-180">Dessutom, precis som hello Azure DB som Cosmos-tjänsten, hello Azure Cosmos DB-emulatorn har endast stöd för säker kommunikation via SSL.</span><span class="sxs-lookup"><span data-stu-id="c9d51-180">Additionally, just as hello Azure Cosmos DB service, hello Azure Cosmos DB Emulator supports only secure communication via SSL.</span></span>

## <a name="running-hello-emulator-on-a-local-network"></a><span data-ttu-id="c9d51-181">Kör hello-emulatorns i ett lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="c9d51-181">Running hello emulator on a local network</span></span>

<span data-ttu-id="c9d51-182">Du kan köra hello emulatorn i ett lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="c9d51-182">You can run hello emulator on a local network.</span></span> <span data-ttu-id="c9d51-183">tooenable nätverksåtkomst, ange hello /AllowNetworkAccess alternativet hello [kommandoraden](#command-line-syntax), vilket kräver att du anger/Key = key_string eller/KeyFile = filnamn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-183">tooenable network access, specify hello /AllowNetworkAccess option at hello [command line](#command-line-syntax), which also requires that you specify /Key=key_string or /KeyFile=file_name.</span></span> <span data-ttu-id="c9d51-184">Du kan använda /GenKeyFile = filnamn toogenerate en fil med en slumpmässig nyckel förskott.</span><span class="sxs-lookup"><span data-stu-id="c9d51-184">You can use /GenKeyFile=file_name toogenerate a file with a random key upfront.</span></span>  <span data-ttu-id="c9d51-185">Sedan kan du skicka den för/KeyFile = filnamn eller/Key = contents_of_file.</span><span class="sxs-lookup"><span data-stu-id="c9d51-185">Then you can pass that too/KeyFile=file_name or /Key=contents_of_file.</span></span>

<span data-ttu-id="c9d51-186">tooenable nätverksåtkomst för hello hello användare bör avstängning hello emulatorn och ta bort hello emulatorn datakatalog (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span><span class="sxs-lookup"><span data-stu-id="c9d51-186">tooenable network access for hello first time hello user should shutdown hello emulator and delete hello emulator’s data directory (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span></span>

## <a name="developing-with-hello-emulator"></a><span data-ttu-id="c9d51-187">Utveckla med hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-187">Developing with hello Emulator</span></span>
<span data-ttu-id="c9d51-188">När du har hello Azure Cosmos DB-Emulator som körs på skrivbordet, kan du använda valfri stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) eller hello [Azure Cosmos DB REST API](/rest/api/documentdb/) toointeract med hello emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-188">Once you have hello Azure Cosmos DB Emulator running on your desktop, you can use any supported [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) or hello [Azure Cosmos DB REST API](/rest/api/documentdb/) toointeract with hello Emulator.</span></span> <span data-ttu-id="c9d51-189">hello Azure Cosmos DB emulatorn innehåller också en inbyggd Data Explorer där du kan skapa samlingar för hello DocumentDB och MongoDB APIs och visa och redigera dokument utan att skriva någon kod.</span><span class="sxs-lookup"><span data-stu-id="c9d51-189">hello Azure Cosmos DB Emulator also includes a built-in Data Explorer that lets you create collections for hello DocumentDB and MongoDB APIs, and view and edit documents without writing any code.</span></span>   

    // Connect toohello Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

<span data-ttu-id="c9d51-190">Om du använder [Azure Cosmos DB Protokollstöd för MongoDB](mongodb-introduction.md), Använd hello följande anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="c9d51-190">If you're using [Azure Cosmos DB protocol support for MongoDB](mongodb-introduction.md), please use hello following connection string:</span></span>

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

<span data-ttu-id="c9d51-191">Du kan använda befintliga verktyg [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-191">You can use existing tools like [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="c9d51-192">Du kan också migrera data mellan hello Azure Cosmos DB-emulatorn och hello Azure DB som Cosmos-tjänsten med hjälp av hello [Azure Cosmos DB Datamigreringsverktyg](https://github.com/azure/azure-documentdb-datamigrationtool).</span><span class="sxs-lookup"><span data-stu-id="c9d51-192">You can also migrate data between hello Azure Cosmos DB Emulator and hello Azure Cosmos DB service using hello [Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).</span></span>

> [!NOTE] 
> <span data-ttu-id="c9d51-193">Om du har startat hello emulator med hello/Key-alternativet kan sedan använda hello genereras nyckel i stället för ”C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw ==”</span><span class="sxs-lookup"><span data-stu-id="c9d51-193">If you have started hello emulator with hello /Key option, then use hello generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="c9d51-194">Med hello Azure Cosmos DB emulatorn som standard kan skapa du too25 enskilda partitionssamlingar eller 1 partitionerad samling.</span><span class="sxs-lookup"><span data-stu-id="c9d51-194">Using hello Azure Cosmos DB emulator, by default, you can create up too25 single partition collections or 1 partitioned collection.</span></span> <span data-ttu-id="c9d51-195">Mer information om hur du ändrar det här värdet finns [hello PartitionCount värdet](#set-partitioncount).</span><span class="sxs-lookup"><span data-stu-id="c9d51-195">For more information about changing this value, see [Setting hello PartitionCount value](#set-partitioncount).</span></span>

## <a name="export-hello-ssl-certificate"></a><span data-ttu-id="c9d51-196">Exportera hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="c9d51-196">Export hello SSL certificate</span></span>

<span data-ttu-id="c9d51-197">.NET-språk och runtime Använd hello Windows certifikatarkiv toosecurely ansluter toohello Azure Cosmos DB lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-197">.NET languages and runtime use hello Windows Certificate Store toosecurely connect toohello Azure Cosmos DB local emulator.</span></span> <span data-ttu-id="c9d51-198">Andra språk har sin egen metod för att hantera och använda certifikat.</span><span class="sxs-lookup"><span data-stu-id="c9d51-198">Other languages have their own method of managing and using certificates.</span></span> <span data-ttu-id="c9d51-199">Java använder sin egen [certifikatarkiv](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) medan Python använder [socket omslutningar](https://docs.python.org/2/library/ssl.html).</span><span class="sxs-lookup"><span data-stu-id="c9d51-199">Java uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) whereas Python uses [socket wrappers](https://docs.python.org/2/library/ssl.html).</span></span>

<span data-ttu-id="c9d51-200">I ordning tooobtain behöver en certifikat-toouse med språk och körningar som inte integreras med hello Windows certifikatarkiv du tooexport den med hjälp av hello Windows Certificate Manager.</span><span class="sxs-lookup"><span data-stu-id="c9d51-200">In order tooobtain a certificate toouse with languages and runtimes that do not integrate with hello Windows Certificate Store you will need tooexport it using hello Windows Certificate Manager.</span></span> <span data-ttu-id="c9d51-201">Du kan starta den genom att köra certlm.msc eller följer hello steg-för-steg-instruktioner i [exportera hello Azure Cosmos DB emulatorn certifikat](./local-emulator-export-ssl-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="c9d51-201">You can start it by running certlm.msc or follow hello step by step instructions in [Export hello Azure Cosmos DB Emulator Certificates](./local-emulator-export-ssl-certificates.md).</span></span> <span data-ttu-id="c9d51-202">När hello Certifikathanteraren körs kan hello öppna hello personliga certifikat som visas nedan och exportera certifikat med hello eget namn ”DocumentDBEmulatorCertificate” som en Base64-kodad X.509 (.cer)-fil.</span><span class="sxs-lookup"><span data-stu-id="c9d51-202">Once hello certificate manager is running, open hello Personal Certificates as shown below and export hello certificate with hello friendly name "DocumentDBEmulatorCertificate" as a BASE-64 encoded X.509 (.cer) file.</span></span>

![Azure DB Cosmos lokala emulatorn SSL-certifikat](./media/local-emulator/database-local-emulator-ssl_certificate.png)

<span data-ttu-id="c9d51-204">hello X.509-certifikat kan importeras till certifikatarkivet för hello Java genom att följa instruktionerna hello i [att lägga till ett certifikat toohello Java Certifikatutfärdarens certifikatarkivet](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span><span class="sxs-lookup"><span data-stu-id="c9d51-204">hello X.509 certificate can be imported into hello Java certificate store by following hello instructions in [Adding a Certificate toohello Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span></span> <span data-ttu-id="c9d51-205">När du har importerat hello certifikatet till certifikatarkivet för hello Java och MongoDB program kommer att vara kan tooconnect toohello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-205">Once hello certificate is imported into hello certificate store, Java and MongoDB applications will be able tooconnect toohello Azure Cosmos DB Emulator.</span></span>

<span data-ttu-id="c9d51-206">När du ansluter toohello emulator från Python och Node.js SDK, är SSL-kontroll inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="c9d51-206">When connecting toohello emulator from Python and Node.js SDKs, SSL verification is disabled.</span></span>

## <span data-ttu-id="c9d51-207"><a id="command-line"></a>Kommandoradsverktyget referens</span><span class="sxs-lookup"><span data-stu-id="c9d51-207"><a id="command-line"></a>Command-line tool reference</span></span>
<span data-ttu-id="c9d51-208">Hello installationsplatsen kan du använda hello kommandoradsverktyget toostart stoppa hello-emulatorn, konfigurera alternativ och utföra andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c9d51-208">From hello installation location, you can use hello command-line toostart and stop hello emulator, configure options, and perform other operations.</span></span>

### <a name="command-line-syntax"></a><span data-ttu-id="c9d51-209">Kommandoradssyntaxen</span><span class="sxs-lookup"><span data-stu-id="c9d51-209">Command-line Syntax</span></span>

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

<span data-ttu-id="c9d51-210">tooview hello listan är typen `CosmosDB.Emulator.exe /?` hello Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c9d51-210">tooview hello list of options, type `CosmosDB.Emulator.exe /?` at hello command prompt.</span></span>

<table>
<tr>
  <td><span data-ttu-id="c9d51-211"><strong>Alternativet</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-211"><strong>Option</strong></span></span></td>
  <td><span data-ttu-id="c9d51-212"><strong>Beskrivning</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-212"><strong>Description</strong></span></span></td>
  <td><span data-ttu-id="c9d51-213"><strong>Kommandot</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-213"><strong>Command</strong></span></span></td>
  <td><span data-ttu-id="c9d51-214"><strong>Argument</strong></span><span class="sxs-lookup"><span data-stu-id="c9d51-214"><strong>Arguments</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-215">[Inga argument]</span><span class="sxs-lookup"><span data-stu-id="c9d51-215">[No arguments]</span></span></td>
  <td><span data-ttu-id="c9d51-216">Startar hello Azure Cosmos DB emulatorn med standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="c9d51-216">Starts up hello Azure Cosmos DB Emulator with default settings.</span></span></td>
  <td><span data-ttu-id="c9d51-217">CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="c9d51-217">CosmosDB.Emulator.exe</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-218">[Hjälp]</span><span class="sxs-lookup"><span data-stu-id="c9d51-218">[Help]</span></span></td>
  <td><span data-ttu-id="c9d51-219">Visar hello lista över kommandoradsargument som stöds.</span><span class="sxs-lookup"><span data-stu-id="c9d51-219">Displays hello list of supported command-line arguments.</span></span></td>
  <td><span data-ttu-id="c9d51-220">CosmosDB.Emulator.exe /?</span><span class="sxs-lookup"><span data-stu-id="c9d51-220">CosmosDB.Emulator.exe /?</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-221">avstängning</span><span class="sxs-lookup"><span data-stu-id="c9d51-221">Shutdown</span></span></td>
  <td><span data-ttu-id="c9d51-222">Stänger ned hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-222">Shuts down hello Azure Cosmos DB Emulator.</span></span></td>
  <td><span data-ttu-id="c9d51-223">CosmosDB.Emulator.exe Shutdown</span><span class="sxs-lookup"><span data-stu-id="c9d51-223">CosmosDB.Emulator.exe /Shutdown</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-224">DataPath</span><span class="sxs-lookup"><span data-stu-id="c9d51-224">DataPath</span></span></td>
  <td><span data-ttu-id="c9d51-225">Anger hello sökväg i vilka toostore datafiler.</span><span class="sxs-lookup"><span data-stu-id="c9d51-225">Specifies hello path in which toostore data files.</span></span> <span data-ttu-id="c9d51-226">Standardvärdet är % LocalAppdata%\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="c9d51-226">Default is %LocalAppdata%\CosmosDBEmulator.</span></span></td>
  <td><span data-ttu-id="c9d51-227">CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-227">CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-228">&lt;DataPath&gt;: en tillgänglig sökväg</span><span class="sxs-lookup"><span data-stu-id="c9d51-228">&lt;datapath&gt;: An accessible path</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-229">Port</span><span class="sxs-lookup"><span data-stu-id="c9d51-229">Port</span></span></td>
  <td><span data-ttu-id="c9d51-230">Anger hello port number toouse för hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-230">Specifies hello port number toouse for hello emulator.</span></span>  <span data-ttu-id="c9d51-231">Standardvärdet är 8081.</span><span class="sxs-lookup"><span data-stu-id="c9d51-231">Default is 8081.</span></span></td>
  <td><span data-ttu-id="c9d51-232">CosmosDB.Emulator.exe/port =&lt;port&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-232">CosmosDB.Emulator.exe /Port=&lt;port&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-233">&lt;port&gt;: enskilda portnummer</span><span class="sxs-lookup"><span data-stu-id="c9d51-233">&lt;port&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-234">MongoPort</span><span class="sxs-lookup"><span data-stu-id="c9d51-234">MongoPort</span></span></td>
  <td><span data-ttu-id="c9d51-235">Anger hello port number toouse för MongoDB kompatibilitet API.</span><span class="sxs-lookup"><span data-stu-id="c9d51-235">Specifies hello port number toouse for MongoDB compatibility API.</span></span> <span data-ttu-id="c9d51-236">Standardvärdet är 10255.</span><span class="sxs-lookup"><span data-stu-id="c9d51-236">Default is 10255.</span></span></td>
  <td><span data-ttu-id="c9d51-237">CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-237">CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-238">&lt;mongoport&gt;: enskilda portnummer</span><span class="sxs-lookup"><span data-stu-id="c9d51-238">&lt;mongoport&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-239">DirectPorts</span><span class="sxs-lookup"><span data-stu-id="c9d51-239">DirectPorts</span></span></td>
  <td><span data-ttu-id="c9d51-240">Anger hello portar toouse för direkt anslutning.</span><span class="sxs-lookup"><span data-stu-id="c9d51-240">Specifies hello ports toouse for direct connectivity.</span></span> <span data-ttu-id="c9d51-241">Standardvärdena är 10251,10252,10253,10254.</span><span class="sxs-lookup"><span data-stu-id="c9d51-241">Defaults are 10251,10252,10253,10254.</span></span></td>
  <td><span data-ttu-id="c9d51-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-243">&lt;directports&gt;: kommaavgränsad lista över 4 portar</span><span class="sxs-lookup"><span data-stu-id="c9d51-243">&lt;directports&gt;: Comma-delimited list of 4 ports</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-244">Nyckel</span><span class="sxs-lookup"><span data-stu-id="c9d51-244">Key</span></span></td>
  <td><span data-ttu-id="c9d51-245">Auktoriseringsnyckeln för hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-245">Authorization key for hello emulator.</span></span> <span data-ttu-id="c9d51-246">Nyckeln måste vara hello Base64-kodning av en vector 64 byte.</span><span class="sxs-lookup"><span data-stu-id="c9d51-246">Key must be hello base-64 encoding of a 64-byte vector.</span></span></td>
  <td><span data-ttu-id="c9d51-247">CosmosDB.Emulator.exe/Key:&lt;nyckel&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-247">CosmosDB.Emulator.exe /Key:&lt;key&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-248">&lt;nyckeln&gt;: nyckeln måste vara hello Base64-kodning av en 64-byte-vektor</span><span class="sxs-lookup"><span data-stu-id="c9d51-248">&lt;key&gt;: Key must be hello base-64 encoding of a 64-byte vector</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-249">EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="c9d51-249">EnableRateLimiting</span></span></td>
  <td><span data-ttu-id="c9d51-250">Anger räntesatsen begäran att begränsa beteendet är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="c9d51-250">Specifies that request rate limiting behavior is enabled.</span></span></td>
  <td><span data-ttu-id="c9d51-251">CosmosDB.Emulator.exe /EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="c9d51-251">CosmosDB.Emulator.exe /EnableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-252">DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="c9d51-252">DisableRateLimiting</span></span></td>
  <td><span data-ttu-id="c9d51-253">Anger räntesatsen begäran att begränsa beteendet är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="c9d51-253">Specifies that request rate limiting behavior is disabled.</span></span></td>
  <td><span data-ttu-id="c9d51-254">CosmosDB.Emulator.exe /DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="c9d51-254">CosmosDB.Emulator.exe /DisableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-255">NoUI</span><span class="sxs-lookup"><span data-stu-id="c9d51-255">NoUI</span></span></td>
  <td><span data-ttu-id="c9d51-256">Visa inte hello emulatorn-användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c9d51-256">Do not show hello emulator user interface.</span></span></td>
  <td><span data-ttu-id="c9d51-257">CosmosDB.Emulator.exe/noui</span><span class="sxs-lookup"><span data-stu-id="c9d51-257">CosmosDB.Emulator.exe /NoUI</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-258">NoExplorer</span><span class="sxs-lookup"><span data-stu-id="c9d51-258">NoExplorer</span></span></td>
  <td><span data-ttu-id="c9d51-259">Visa inte dokumentutforskaren vid start.</span><span class="sxs-lookup"><span data-stu-id="c9d51-259">Don't show document explorer on startup.</span></span></td>
  <td><span data-ttu-id="c9d51-260">CosmosDB.Emulator.exe /NoExplorer</span><span class="sxs-lookup"><span data-stu-id="c9d51-260">CosmosDB.Emulator.exe /NoExplorer</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-261">PartitionCount</span><span class="sxs-lookup"><span data-stu-id="c9d51-261">PartitionCount</span></span></td>
  <td><span data-ttu-id="c9d51-262">Anger hello maxantalet partitionerade samlingar.</span><span class="sxs-lookup"><span data-stu-id="c9d51-262">Specifies hello maximum number of partitioned collections.</span></span> <span data-ttu-id="c9d51-263">Se [ändra hello antal samlingar](#set-partitioncount) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c9d51-263">See [Change hello number of collections](#set-partitioncount) for more information.</span></span></td>
  <td><span data-ttu-id="c9d51-264">CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-264">CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-265">&lt;partitioncount&gt;: maximalt antal tillåtna enskilda partitionssamlingar.</span><span class="sxs-lookup"><span data-stu-id="c9d51-265">&lt;partitioncount&gt;: Maximum number of allowed single partition collections.</span></span> <span data-ttu-id="c9d51-266">Standardvärdet är 25.</span><span class="sxs-lookup"><span data-stu-id="c9d51-266">Default is 25.</span></span> <span data-ttu-id="c9d51-267">Högsta tillåtna är 250.</span><span class="sxs-lookup"><span data-stu-id="c9d51-267">Maximum allowed is 250.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-268">DefaultPartitionCount</span><span class="sxs-lookup"><span data-stu-id="c9d51-268">DefaultPartitionCount</span></span></td>
  <td><span data-ttu-id="c9d51-269">Anger hello standardantalet partitioner för en partitionerad samling.</span><span class="sxs-lookup"><span data-stu-id="c9d51-269">Specifies hello default number of partitions for a partitioned collection.</span></span></td>
  <td><span data-ttu-id="c9d51-270">CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-270">CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-271">&lt;defaultpartitioncount&gt; standardvärdet är 25.</span><span class="sxs-lookup"><span data-stu-id="c9d51-271">&lt;defaultpartitioncount&gt; Default is 25.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-272">AllowNetworkAccess</span><span class="sxs-lookup"><span data-stu-id="c9d51-272">AllowNetworkAccess</span></span></td>
  <td><span data-ttu-id="c9d51-273">Aktiverar åtkomst till toohello emulatorn över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="c9d51-273">Enables access toohello emulator over a network.</span></span> <span data-ttu-id="c9d51-274">Du måste också ange/Key =&lt;key_string&gt; eller/KeyFile =&lt;file_name&gt; tooenable nätverksåtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9d51-274">You must also pass /Key=&lt;key_string&gt; or /KeyFile=&lt;file_name&gt; tooenable network access.</span></span></td>
  <td><span data-ttu-id="c9d51-275">CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-275">CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;</span></span><br><br><span data-ttu-id="c9d51-276">eller</span><span class="sxs-lookup"><span data-stu-id="c9d51-276">or</span></span><br><br><span data-ttu-id="c9d51-277">CosmosDB.Emulator.exe /AllowNetworkAccess/KeyFile =&lt;filnamn&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-277">CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-278">NoFirewall</span><span class="sxs-lookup"><span data-stu-id="c9d51-278">NoFirewall</span></span></td>
  <td><span data-ttu-id="c9d51-279">Justera inte brandväggsregler när /AllowNetworkAccess används.</span><span class="sxs-lookup"><span data-stu-id="c9d51-279">Don't adjust firewall rules when /AllowNetworkAccess is used.</span></span></td>
  <td><span data-ttu-id="c9d51-280">CosmosDB.Emulator.exe /NoFirewall</span><span class="sxs-lookup"><span data-stu-id="c9d51-280">CosmosDB.Emulator.exe /NoFirewall</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-281">GenKeyFile</span><span class="sxs-lookup"><span data-stu-id="c9d51-281">GenKeyFile</span></span></td>
  <td><span data-ttu-id="c9d51-282">Skapa en ny auktoriserings-nyckel och spara toohello angivna filen.</span><span class="sxs-lookup"><span data-stu-id="c9d51-282">Generate a new authorization key and save toohello specified file.</span></span> <span data-ttu-id="c9d51-283">hello genereras nyckeln kan användas med hello/Key eller/KeyFile alternativ.</span><span class="sxs-lookup"><span data-stu-id="c9d51-283">hello generated key can be used with hello /Key or /KeyFile options.</span></span></td>
  <td><span data-ttu-id="c9d51-284">CosmosDB.Emulator.exe /GenKeyFile =&lt;sökväg tookey fil&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-284">CosmosDB.Emulator.exe  /GenKeyFile=&lt;path tookey file&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-285">Konsekvens</span><span class="sxs-lookup"><span data-stu-id="c9d51-285">Consistency</span></span></td>
  <td><span data-ttu-id="c9d51-286">Ange hello konsekvenskontroll standardnivå för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="c9d51-286">Set hello default consistency level for hello account.</span></span></td>
  <td><span data-ttu-id="c9d51-287">CosmosDB.Emulator.exe /Consistency =&lt;konsekvenskontroll&gt;</span><span class="sxs-lookup"><span data-stu-id="c9d51-287">CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</span></span></td>
  <td><span data-ttu-id="c9d51-288">&lt;konsekvenskontroll&gt;: värdet måste vara något av följande hello [konsekvensnivåer](consistency-levels.md): Session starka, Eventual eller BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="c9d51-288">&lt;consistency&gt;: Value must be one of hello following [consistency levels](consistency-levels.md): Session, Strong, Eventual, or BoundedStaleness.</span></span>  <span data-ttu-id="c9d51-289">hello standardvärdet är Session.</span><span class="sxs-lookup"><span data-stu-id="c9d51-289">hello default value is Session.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="c9d51-290">?</span><span class="sxs-lookup"><span data-stu-id="c9d51-290">?</span></span></td>
  <td><span data-ttu-id="c9d51-291">Visa hello-meddelande för hjälp.</span><span class="sxs-lookup"><span data-stu-id="c9d51-291">Show hello help message.</span></span></td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-hello-azure-cosmos-db-emulator-and-azure-cosmos-db"></a><span data-ttu-id="c9d51-292">Skillnader mellan hello Azure Cosmos DB-emulatorn och Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c9d51-292">Differences between hello Azure Cosmos DB Emulator and Azure Cosmos DB</span></span> 
<span data-ttu-id="c9d51-293">Eftersom hello Azure Cosmos DB-emulatorn är en emulerade miljö som körs på en lokal developer-arbetsstation, finns det vissa skillnader i funktionalitet mellan hello-emulatorn och ett Azure DB som Cosmos-konto i molnet hello:</span><span class="sxs-lookup"><span data-stu-id="c9d51-293">Because hello Azure Cosmos DB Emulator provides an emulated environment running on a local developer workstation, there are some differences in functionality between hello emulator and an Azure Cosmos DB account in hello cloud:</span></span>

* <span data-ttu-id="c9d51-294">hello Azure Cosmos DB emulatorn stöder bara ett fast konto och en välkänd huvudnyckel.</span><span class="sxs-lookup"><span data-stu-id="c9d51-294">hello Azure Cosmos DB Emulator supports only a single fixed account and a well-known master key.</span></span>  <span data-ttu-id="c9d51-295">Det går inte att nyckeln återskapas i hello Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-295">Key regeneration is not possible in hello Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="c9d51-296">hello Azure Cosmos DB-emulatorn är inte en skalbar tjänst och stöder inte ett stort antal samlingar.</span><span class="sxs-lookup"><span data-stu-id="c9d51-296">hello Azure Cosmos DB Emulator is not a scalable service and will not support a large number of collections.</span></span>
* <span data-ttu-id="c9d51-297">hello Azure Cosmos DB emulatorn simulerar inte olika [Azure Cosmos DB konsekvensnivåer](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="c9d51-297">hello Azure Cosmos DB Emulator does not simulate different [Azure Cosmos DB consistency levels](consistency-levels.md).</span></span>
* <span data-ttu-id="c9d51-298">hello Azure Cosmos DB emulatorn simulerar inte [flera regioner replikering](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="c9d51-298">hello Azure Cosmos DB Emulator does not simulate [multi-region replication](distribute-data-globally.md).</span></span>
* <span data-ttu-id="c9d51-299">hello Azure Cosmos DB emulatorn stöder inte hello service kvoten åsidosättningar som är tillgängliga i hello Azure DB som Cosmos-tjänsten (t.ex. dokument storleksgränser, ökad partitionerad samling lagring).</span><span class="sxs-lookup"><span data-stu-id="c9d51-299">hello Azure Cosmos DB Emulator does not support hello service quota overrides that are available in hello Azure Cosmos DB service (e.g. document size limits, increased partitioned collection storage).</span></span>
* <span data-ttu-id="c9d51-300">Som ditt exemplar av hello Azure Cosmos DB emulatorn inte eventuellt upp toodate med hello de senaste ändringarna med hello Azure DB som Cosmos-tjänsten, ta [Azure Cosmos DB kapacitetsplaneringsverktyg](https://www.documentdb.com/capacityplanner) tooaccurately uppskattning produktion genomströmning (RUs) behoven för ditt program.</span><span class="sxs-lookup"><span data-stu-id="c9d51-300">As your copy of hello Azure Cosmos DB Emulator might not be up toodate with hello most recent changes with hello Azure Cosmos DB service, please [Azure Cosmos DB capacity planner](https://www.documentdb.com/capacityplanner) tooaccurately estimate production throughput (RUs) needs of your application.</span></span>

## <span data-ttu-id="c9d51-301"><a id="set-partitioncount"></a>Ändra hello antal samlingar</span><span class="sxs-lookup"><span data-stu-id="c9d51-301"><a id="set-partitioncount"></a>Change hello number of collections</span></span>

<span data-ttu-id="c9d51-302">Du kan skapa upp too25 enskilda partitionssamlingar eller 1 partitionerad samling med hello Azure Cosmos DB emulatorn som standard.</span><span class="sxs-lookup"><span data-stu-id="c9d51-302">By default, you can create up too25 single partition collections, or 1 partitioned collection using hello Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="c9d51-303">Genom att ändra hello **PartitionCount** värde, kan du skapa too250 enskilda partitionssamlingar eller 10 partitionerade samlingar eller en kombination av hello två som innehåller högst 250 enskild partitionerar (där 1 partitionerad samlingen = 25 enskild partition samling).</span><span class="sxs-lookup"><span data-stu-id="c9d51-303">By modifying hello **PartitionCount** value, you can create up too250 single partition collections or 10 partitioned collections, or any combination of hello two that does not exceed 250 single partitions (where 1 partitioned collection = 25 single partition collection).</span></span>

<span data-ttu-id="c9d51-304">Om du försöker toocreate en samling när hello aktuella partitionsantal har överskridits, utlöser hello emulatorn ett ServiceUnavailable undantag med hello efter meddelande.</span><span class="sxs-lookup"><span data-stu-id="c9d51-304">If you attempt toocreate a collection after hello current partition count has been exceeded, hello emulator throws a ServiceUnavailable exception, with hello following message.</span></span>

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    toobring more and more capacity online, and encourage you tootry again. 
    Please do not hesitate tooemail docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

<span data-ttu-id="c9d51-305">toochange hello antal samlingar tillgängliga toohello Azure Cosmos DB emulatorn hello följande:</span><span class="sxs-lookup"><span data-stu-id="c9d51-305">toochange hello number of collections available toohello Azure Cosmos DB Emulator, do hello following:</span></span>

1. <span data-ttu-id="c9d51-306">Ta bort alla lokala data i Azure Cosmos DB-emulatorn genom att högerklicka på hello **Azure Cosmos DB emulatorn** ikon på hello systemfältet och sedan klicka på **återställa Data...** .</span><span class="sxs-lookup"><span data-stu-id="c9d51-306">Delete all local Azure Cosmos DB Emulator data by right-clicking hello **Azure Cosmos DB Emulator** icon on hello system tray, and then clicking **Reset Data…**.</span></span>
2. <span data-ttu-id="c9d51-307">Ta bort alla emulatorn data i den här mappen C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="c9d51-307">Delete all emulator data in this folder C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span></span>
3. <span data-ttu-id="c9d51-308">Avsluta alla öppna instanser genom att högerklicka på hello **Azure Cosmos DB emulatorn** ikon på hello systemfältet och sedan klicka på **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="c9d51-308">Exit all open instances by right-clicking hello **Azure Cosmos DB Emulator** icon on hello system tray, and then clicking **Exit**.</span></span> <span data-ttu-id="c9d51-309">Det kan ta ett tag för alla instanser tooexit.</span><span class="sxs-lookup"><span data-stu-id="c9d51-309">It may take a minute for all instances tooexit.</span></span>
4. <span data-ttu-id="c9d51-310">Installera hello senaste versionen av hello [Azure Cosmos DB emulatorn](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="c9d51-310">Install hello latest version of hello [Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator).</span></span>
5. <span data-ttu-id="c9d51-311">Starta hello emulator med hello PartitionCount flaggan genom att ange ett värde < = 250.</span><span class="sxs-lookup"><span data-stu-id="c9d51-311">Launch hello emulator with hello PartitionCount flag by setting a value <= 250.</span></span> <span data-ttu-id="c9d51-312">Till exempel: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span><span class="sxs-lookup"><span data-stu-id="c9d51-312">For example: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c9d51-313">Felsökning</span><span class="sxs-lookup"><span data-stu-id="c9d51-313">Troubleshooting</span></span>

<span data-ttu-id="c9d51-314">Använd hello följande tips toohelp felsöka problem som kan uppstå med hello Azure DB som Cosmos-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="c9d51-314">Use hello following tips toohelp troubleshoot issues you encounter with hello Azure Cosmos DB emulator:</span></span>

- <span data-ttu-id="c9d51-315">Om du har installerat en ny version av hello emulatorn och fel har uppstått, se till att du återställer data.</span><span class="sxs-lookup"><span data-stu-id="c9d51-315">If you installed a new version of hello Emulator and are experiencing errors, ensure you reset your data.</span></span> <span data-ttu-id="c9d51-316">Du kan återställa dina data genom att högerklicka på ikonen för hello Azure Cosmos DB emulatorn på hello systemfältet och sedan klicka på Återställ Data...</span><span class="sxs-lookup"><span data-stu-id="c9d51-316">You can reset your data by right-clicking hello Azure Cosmos DB Emulator icon on hello system tray, and then clicking Reset Data….</span></span> <span data-ttu-id="c9d51-317">Om detta inte löser hello fel, kan du avinstallera och installera hello app.</span><span class="sxs-lookup"><span data-stu-id="c9d51-317">If that does not fix hello errors, you can uninstall and reinstall hello app.</span></span> <span data-ttu-id="c9d51-318">Se [avinstallera hello lokala emulatorn](#uninstall) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="c9d51-318">See [Uninstall hello local emulator](#uninstall) for instructions.</span></span>

- <span data-ttu-id="c9d51-319">Om hello Azure Cosmos DB emulatorn krascher, samla in filer med felsökningsdumpar från c:\Users\user_name\AppData\Local\CrashDumps mappen komprimeras och bifoga dem tooan e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c9d51-319">If hello Azure Cosmos DB emulator crashes, collect dump files from c:\Users\user_name\AppData\Local\CrashDumps folder, compress them, and attach them tooan email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="c9d51-320">Om det uppstår krascher i CosmosDB.StartupEntryPoint.exe, kör du följande kommando från en kommandotolk för admin hello:`lodctr /R`</span><span class="sxs-lookup"><span data-stu-id="c9d51-320">If you experience crashes in CosmosDB.StartupEntryPoint.exe, run hello following command from an admin command prompt: `lodctr /R`</span></span> 

- <span data-ttu-id="c9d51-321">Om du stöter på ett anslutningsproblem, [samla in spårningsfiler](#trace-files), komprimeras och bifogar dem tooan e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c9d51-321">If you encounter a connectivity issue, [collect trace files](#trace-files), compress them, and attach them tooan email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="c9d51-322">Om du får en **tjänsten är inte tillgänglig** visas, hello emulatorn vara skadad tooinitialize hello network-stacken.</span><span class="sxs-lookup"><span data-stu-id="c9d51-322">If you receive a **Service Unavailable** message, hello emulator might be failing tooinitialize hello network stack.</span></span> <span data-ttu-id="c9d51-323">Kontrollera toosee om du har hello Pulse secure klient- eller Juniper nätverk klienten är installerad, som filterdrivrutiner nätverket kan orsaka hello problem.</span><span class="sxs-lookup"><span data-stu-id="c9d51-323">Check toosee if you have hello Pulse secure client or Juniper networks client installed, as their network filter drivers may cause hello problem.</span></span> <span data-ttu-id="c9d51-324">Avinstallera drivrutiner från tredje part nätverket filter vanligtvis åtgärdas hello problem.</span><span class="sxs-lookup"><span data-stu-id="c9d51-324">Uninstalling third party network filter drivers typically fixes hello issue.</span></span>

### <span data-ttu-id="c9d51-325"><a id="trace-files"></a>Samla in spårningsfiler</span><span class="sxs-lookup"><span data-stu-id="c9d51-325"><a id="trace-files"></a>Collect trace files</span></span>

<span data-ttu-id="c9d51-326">toocollect felsökning spårning, kör följande kommandon från en administrativ kommandotolk hello:</span><span class="sxs-lookup"><span data-stu-id="c9d51-326">toocollect debugging traces, run hello following commands from an administrative command prompt:</span></span>

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. <span data-ttu-id="c9d51-327">`CosmosDB.Emulator.exe /shutdown`.</span><span class="sxs-lookup"><span data-stu-id="c9d51-327">`CosmosDB.Emulator.exe /shutdown`.</span></span> <span data-ttu-id="c9d51-328">Titta på hello system fack toomake att hello programmet har avslutats kan det ta ett tag.</span><span class="sxs-lookup"><span data-stu-id="c9d51-328">Watch hello system tray toomake sure hello program has shut down, it may take a minute.</span></span> <span data-ttu-id="c9d51-329">Du kan även klicka **avsluta** i användargränssnittet för hello Azure DB som Cosmos-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c9d51-329">You can also just click **Exit** in hello Azure Cosmos DB emulator user interface.</span></span>
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. <span data-ttu-id="c9d51-330">Återskapa hello problemet.</span><span class="sxs-lookup"><span data-stu-id="c9d51-330">Reproduce hello problem.</span></span> <span data-ttu-id="c9d51-331">Om Data Explorer inte fungerar kan behöver du bara toowait för hello webbläsare tooopen för några sekunder toocatch hello-fel.</span><span class="sxs-lookup"><span data-stu-id="c9d51-331">If Data Explorer is not working, you only need toowait for hello browser tooopen for a few seconds toocatch hello error.</span></span>
5. `CosmosDB.Emulator.exe /stoptraces`
6. <span data-ttu-id="c9d51-332">Navigera för`%ProgramFiles%\Azure Cosmos DB Emulator` och hitta hello docdbemulator_000001.etl filen.</span><span class="sxs-lookup"><span data-stu-id="c9d51-332">Navigate too`%ProgramFiles%\Azure Cosmos DB Emulator` and find hello docdbemulator_000001.etl file.</span></span>
7. <span data-ttu-id="c9d51-333">Skicka hello etl-filen tillsammans med reproducera anvisningar för[ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) för felsökning.</span><span class="sxs-lookup"><span data-stu-id="c9d51-333">Send hello .etl file along with repro steps too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) for debugging.</span></span>

### <span data-ttu-id="c9d51-334"><a id="uninstall"></a>Avinstallera hello lokala emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-334"><a id="uninstall"></a>Uninstall hello local Emulator</span></span>

1. <span data-ttu-id="c9d51-335">Avsluta alla öppna instanser av hello lokala emulatorn genom att högerklicka på ikonen för hello Azure Cosmos DB emulatorn på hello systemfältet och klicka på Avsluta.</span><span class="sxs-lookup"><span data-stu-id="c9d51-335">Exit all open instances of hello local Emulator by right-clicking hello Azure Cosmos DB Emulator icon on hello system tray, and then clicking Exit.</span></span> <span data-ttu-id="c9d51-336">Det kan ta ett tag för alla instanser tooexit.</span><span class="sxs-lookup"><span data-stu-id="c9d51-336">It may take a minute for all instances tooexit.</span></span>
2. <span data-ttu-id="c9d51-337">Skriv i sökrutan för hello Windows **appar och funktioner** och klicka på hello **appar och funktioner (systeminställningar)** resultat.</span><span class="sxs-lookup"><span data-stu-id="c9d51-337">In hello Windows search box, type **Apps & features** and click on hello **Apps & features (System settings)** result.</span></span>
3. <span data-ttu-id="c9d51-338">Rulla för i hello listan över appar**Azure Cosmos DB emulatorn**, markerar du den, klickar du på **avinstallera**, bekräfta och klicka på **avinstallera** igen.</span><span class="sxs-lookup"><span data-stu-id="c9d51-338">In hello list of apps, scroll too**Azure Cosmos DB Emulator**, select it, click **Uninstall**, then confirm and click **Uninstall** again.</span></span>
4. <span data-ttu-id="c9d51-339">När hello appen avinstalleras, navigera tooC:\Users\<användare > \AppData\Local\CosmosDBEmulator och ta bort hello-mapp.</span><span class="sxs-lookup"><span data-stu-id="c9d51-339">When hello app is uninstalled, navigate tooC:\Users\<user>\AppData\Local\CosmosDBEmulator and delete hello folder.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c9d51-340">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9d51-340">Next steps</span></span>

<span data-ttu-id="c9d51-341">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="c9d51-341">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c9d51-342">Installerat hello lokala emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-342">Installed hello local Emulator</span></span>
> * <span data-ttu-id="c9d51-343">SLUMP hello Emulator på Docker för Windows</span><span class="sxs-lookup"><span data-stu-id="c9d51-343">Rand hello Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="c9d51-344">Autentiserade begäranden</span><span class="sxs-lookup"><span data-stu-id="c9d51-344">Authenticated requests</span></span>
> * <span data-ttu-id="c9d51-345">Används hello Data Explorer i hello emulatorn</span><span class="sxs-lookup"><span data-stu-id="c9d51-345">Used hello Data Explorer in hello Emulator</span></span>
> * <span data-ttu-id="c9d51-346">Exporterade SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="c9d51-346">Exported SSL certificates</span></span>
> * <span data-ttu-id="c9d51-347">Kallas hello Emulator från hello kommandorad</span><span class="sxs-lookup"><span data-stu-id="c9d51-347">Called hello Emulator from hello command line</span></span>
> * <span data-ttu-id="c9d51-348">Insamlade spårningsfiler</span><span class="sxs-lookup"><span data-stu-id="c9d51-348">Collected trace files</span></span>

<span data-ttu-id="c9d51-349">I kursen får du har lärt dig hur toouse hello lokala Emulator för kostnadsfria lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="c9d51-349">In this tutorial, you've learned how toouse hello local Emulator for free local development.</span></span> <span data-ttu-id="c9d51-350">Du kan nu fortsätta toohello nästa kurs och lära dig hur tooexport emulatorn SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="c9d51-350">You can now proceed toohello next tutorial and learn how tooexport Emulator SSL certificates.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="c9d51-351">Exportera hello Azure Cosmos DB emulatorn certifikat</span><span class="sxs-lookup"><span data-stu-id="c9d51-351">Export hello Azure Cosmos DB Emulator certificates</span></span>](local-emulator-export-ssl-certificates.md)
