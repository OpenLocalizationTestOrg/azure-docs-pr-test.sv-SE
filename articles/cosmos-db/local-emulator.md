---
title: Utveckla lokalt med Azure Cosmos DB-emulatorn | Microsoft Docs
description: "Med Azure Cosmos DB-emulatorn kan du utveckla och testa programmet lokalt för gratis, utan att skapa en Azure-prenumeration."
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
ms.openlocfilehash: a0f6a845a345ebd4ef0a58abf4934ce400103109
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a><span data-ttu-id="b5f87-104">Använd Azure Cosmos DB-emulatorn för lokal utveckling och testning</span><span class="sxs-lookup"><span data-stu-id="b5f87-104">Use the Azure Cosmos DB Emulator for local development and testing</span></span>

<table>
<tr>
  <td><span data-ttu-id="b5f87-105"><strong>Binärfiler</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-105"><strong>Binaries</strong></span></span></td>
  <td>[<span data-ttu-id="b5f87-106">Ladda ned MSI</span><span class="sxs-lookup"><span data-stu-id="b5f87-106">Download MSI</span></span>](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-107"><strong>Docker</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-107"><strong>Docker</strong></span></span></td>
  <td>[<span data-ttu-id="b5f87-108">Docker-hubb</span><span class="sxs-lookup"><span data-stu-id="b5f87-108">Docker Hub</span></span>](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-109"><strong>Docker-källa</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-109"><strong>Docker source</strong></span></span></td>
  <td>[<span data-ttu-id="b5f87-110">Github</span><span class="sxs-lookup"><span data-stu-id="b5f87-110">Github</span></span>](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
<span data-ttu-id="b5f87-111">Azure-emulatorn Cosmos DB tillhandahåller en lokal miljö som emulerar Azure DB som Cosmos-tjänsten för utveckling.</span><span class="sxs-lookup"><span data-stu-id="b5f87-111">The Azure Cosmos DB Emulator provides a local environment that emulates the Azure Cosmos DB service for development purposes.</span></span> <span data-ttu-id="b5f87-112">Med Azure Cosmos DB-emulatorn kan du utveckla och testa programmet lokalt, utan att skapa en Azure-prenumeration eller kostnader.</span><span class="sxs-lookup"><span data-stu-id="b5f87-112">Using the Azure Cosmos DB Emulator, you can develop and test your application locally, without creating an Azure subscription or incurring any costs.</span></span> <span data-ttu-id="b5f87-113">När du är nöjd med hur programmet fungerar i Azure Cosmos DB-emulatorn kan växla du till med ett Azure DB som Cosmos-konto i molnet.</span><span class="sxs-lookup"><span data-stu-id="b5f87-113">When you're satisfied with how your application is working in the Azure Cosmos DB Emulator, you can switch to using an Azure Cosmos DB account in the cloud.</span></span>

<span data-ttu-id="b5f87-114">Den här artikeln omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="b5f87-114">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="b5f87-115">Installerar emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-115">Installing the Emulator</span></span>
> * <span data-ttu-id="b5f87-116">Kör emulatorn på Docker för Windows</span><span class="sxs-lookup"><span data-stu-id="b5f87-116">Running the Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="b5f87-117">Autentisering av förfrågningar</span><span class="sxs-lookup"><span data-stu-id="b5f87-117">Authenticating requests</span></span>
> * <span data-ttu-id="b5f87-118">Med hjälp av Data Explorer i emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-118">Using the Data Explorer in the Emulator</span></span>
> * <span data-ttu-id="b5f87-119">Exportera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="b5f87-119">Exporting SSL certificates</span></span>
> * <span data-ttu-id="b5f87-120">Anropar emulatorn från kommandoraden</span><span class="sxs-lookup"><span data-stu-id="b5f87-120">Calling the Emulator from the command line</span></span>
> * <span data-ttu-id="b5f87-121">Samla in spårningsfiler</span><span class="sxs-lookup"><span data-stu-id="b5f87-121">Collecting trace files</span></span>

<span data-ttu-id="b5f87-122">Vi rekommenderar att komma igång med att titta på nedanstående video, där Kirill Gavrylyuk visar hur du kommer igång med Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-122">We recommend getting started by watching the following video, where Kirill Gavrylyuk shows how to get started with the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="b5f87-123">Observera att videon refererar till emulatorn som DocumentDB-emulatorn, men själva verktyget har bytt namn Azure Cosmos DB-emulatorn sedan in videon.</span><span class="sxs-lookup"><span data-stu-id="b5f87-123">Note that the video refers to the emulator as the DocumentDB Emulator, but the tool itself has been renamed the Azure Cosmos DB Emulator since taping the video.</span></span> <span data-ttu-id="b5f87-124">All information i videon är korrekt för Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-124">All information in the video is still accurate for the Azure Cosmos DB Emulator.</span></span> 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-the-emulator-works"></a><span data-ttu-id="b5f87-125">Så här fungerar emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-125">How the Emulator works</span></span>
<span data-ttu-id="b5f87-126">Azure-emulatorn Cosmos DB ger en hög återgivning emulering av tjänsten Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b5f87-126">The Azure Cosmos DB Emulator provides a high-fidelity emulation of the Azure Cosmos DB service.</span></span> <span data-ttu-id="b5f87-127">Den stöder identiska funktioner som Azure Cosmos DB, inklusive stöd för att skapa och hämtning av JSON-dokument, etablering och skalning samlingar och köra lagrade procedurer och utlösare.</span><span class="sxs-lookup"><span data-stu-id="b5f87-127">It supports identical functionality as Azure Cosmos DB, including support for creating and querying JSON documents, provisioning and scaling collections, and executing stored procedures and triggers.</span></span> <span data-ttu-id="b5f87-128">Du kan utveckla och testa program med hjälp av Azure Cosmos DB emulatorn och distribuera dem till Azure på global nivå genom att bara göra en enda konfigurationen av anslutningens slutpunkt för Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b5f87-128">You can develop and test applications using the Azure Cosmos DB Emulator, and deploy them to Azure at global scale by just making a single configuration change to the connection endpoint for Azure Cosmos DB.</span></span>

<span data-ttu-id="b5f87-129">Medan vi har skapat en lokal emulering hög återgivning av tjänsten faktiska Azure Cosmos DB är implementeringen av Azure Cosmos DB-emulatorn än tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b5f87-129">While we created a high-fidelity local emulation of the actual Azure Cosmos DB service, the implementation of the Azure Cosmos DB Emulator is different than that of the service.</span></span> <span data-ttu-id="b5f87-130">Till exempel använder Azure Cosmos DB-emulatorn standard OS-komponenter, till exempel det lokala filsystemet för beständighet och HTTPS-protokoll-stacken för anslutning.</span><span class="sxs-lookup"><span data-stu-id="b5f87-130">For example, the Azure Cosmos DB Emulator uses standard OS components such as the local file system for persistence, and HTTPS protocol stack for connectivity.</span></span> <span data-ttu-id="b5f87-131">Detta innebär att vissa funktioner som förlitar sig på Azure-infrastrukturen som globala replikering, en siffra millisekunds fördröjning för läsning/skrivning och justerbara konsekvensnivåer inte är tillgängliga via Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-131">This means that some functionality that relies on Azure infrastructure like global replication, single-digit millisecond latency for reads/writes, and tunable consistency levels are not available via the Azure Cosmos DB Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="b5f87-132">Data Explorer i emulatorn stöder endast skapandet av DocumentDB API samlingar och MongoDB samlingar just nu.</span><span class="sxs-lookup"><span data-stu-id="b5f87-132">At this time the Data Explorer in the emulator only supports the creation of DocumentDB API collections and MongoDB collections.</span></span> <span data-ttu-id="b5f87-133">Data Explorer i emulatorn stöder för närvarande inte att skapa tabeller och diagram.</span><span class="sxs-lookup"><span data-stu-id="b5f87-133">The Data Explorer in the emulator does not currently support the creation of tables and graphs.</span></span> 

## <a name="system-requirements"></a><span data-ttu-id="b5f87-134">Systemkrav</span><span class="sxs-lookup"><span data-stu-id="b5f87-134">System requirements</span></span>
<span data-ttu-id="b5f87-135">Azure Cosmos DB-emulatorn har följande krav för maskinvara och programvara:</span><span class="sxs-lookup"><span data-stu-id="b5f87-135">The Azure Cosmos DB Emulator has the following hardware and software requirements:</span></span>

* <span data-ttu-id="b5f87-136">Programvarukrav</span><span class="sxs-lookup"><span data-stu-id="b5f87-136">Software requirements</span></span>
  * <span data-ttu-id="b5f87-137">Windows Server 2012 R2, Windows Server 2016 eller Windows 10</span><span class="sxs-lookup"><span data-stu-id="b5f87-137">Windows Server 2012 R2, Windows Server 2016, or Windows 10</span></span>
*   <span data-ttu-id="b5f87-138">Minsta maskinvarukrav</span><span class="sxs-lookup"><span data-stu-id="b5f87-138">Minimum Hardware requirements</span></span>
  * <span data-ttu-id="b5f87-139">2 GB RAM-MINNE</span><span class="sxs-lookup"><span data-stu-id="b5f87-139">2 GB RAM</span></span>
  * <span data-ttu-id="b5f87-140">10 GB ledigt hårddiskutrymme</span><span class="sxs-lookup"><span data-stu-id="b5f87-140">10 GB available hard disk space</span></span>

## <a name="installation"></a><span data-ttu-id="b5f87-141">Installation</span><span class="sxs-lookup"><span data-stu-id="b5f87-141">Installation</span></span>
<span data-ttu-id="b5f87-142">Du kan hämta och installera Azure Cosmos-DB-emulatorn från den [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="b5f87-142">You can download and install the Azure Cosmos DB Emulator from the [Microsoft Download Center](https://aka.ms/cosmosdb-emulator).</span></span> 

> [!NOTE]
> <span data-ttu-id="b5f87-143">Om du vill installera, konfigurera och köra Azure Cosmos DB-emulatorn, måste du ha administratörsbehörighet på datorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-143">To install, configure, and run the Azure Cosmos DB Emulator, you must have administrative privileges on the computer.</span></span>

## <a name="running-on-docker-for-windows"></a><span data-ttu-id="b5f87-144">Körs på Docker för Windows</span><span class="sxs-lookup"><span data-stu-id="b5f87-144">Running on Docker for Windows</span></span>

<span data-ttu-id="b5f87-145">Azure Cosmos DB-emulatorn kan köras på Docker för Windows.</span><span class="sxs-lookup"><span data-stu-id="b5f87-145">The Azure Cosmos DB Emulator can be run on Docker for Windows.</span></span> <span data-ttu-id="b5f87-146">Emulatorn fungerar inte på Docker för Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="b5f87-146">The Emulator does not work on Docker for Oracle Linux.</span></span>

<span data-ttu-id="b5f87-147">När du har [Docker för Windows](https://www.docker.com/docker-windows) installerat kan du dra emulatorn avbildningen från Docker-hubben genom att köra följande kommando från dina Favoriter shell (cmd.exe, PowerShell, etc.).</span><span class="sxs-lookup"><span data-stu-id="b5f87-147">Once you have [Docker for Windows](https://www.docker.com/docker-windows) installed, you can pull the Emulator image from Docker Hub by running the following command from your favorite shell (cmd.exe, PowerShell, etc.).</span></span>

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
<span data-ttu-id="b5f87-148">Kör följande kommandon för att starta avbildningen.</span><span class="sxs-lookup"><span data-stu-id="b5f87-148">To start the image, run the following commands.</span></span>

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

<span data-ttu-id="b5f87-149">Svaret ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="b5f87-149">The response looks similar to the following:</span></span>

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

<span data-ttu-id="b5f87-150">Stänger det interaktiva gränssnittet när emulatorn har startat stängs av emulatorn behållare.</span><span class="sxs-lookup"><span data-stu-id="b5f87-150">Closing the interactive shell once the Emulator has been started will shutdown the Emulator’s container.</span></span>

<span data-ttu-id="b5f87-151">Använd slutpunkt och huvudnyckeln i från svaret i din klient och importera SSL-certifikatet till värden.</span><span class="sxs-lookup"><span data-stu-id="b5f87-151">Use the endpoint and master key in from the response in your client and import the SSL certificate into your host.</span></span> <span data-ttu-id="b5f87-152">Om du vill importera SSL-certifikatet, gör du följande från en kommandotolk för administratörer:</span><span class="sxs-lookup"><span data-stu-id="b5f87-152">To import the SSL certificate, do the following from an admin command prompt:</span></span>

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-the-emulator"></a><span data-ttu-id="b5f87-153">Starta emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-153">Start the Emulator</span></span>

<span data-ttu-id="b5f87-154">Klicka på knappen Start eller tryck på Windows-tangenten för att starta Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-154">To start the Azure Cosmos DB Emulator, select the Start button or press the Windows key.</span></span> <span data-ttu-id="b5f87-155">Börja skriva **Azure Cosmos DB emulatorn**, och välj emulatorn från listan med program.</span><span class="sxs-lookup"><span data-stu-id="b5f87-155">Begin typing **Azure Cosmos DB Emulator**, and select the emulator from the list of applications.</span></span> 

![Klicka på knappen Start eller tryck på Windows-tangenten, börja skriva ** Azure Cosmos DB emulatorn ** och välj emulator från listan med program](./media/local-emulator/database-local-emulator-start.png)

<span data-ttu-id="b5f87-157">När emulatorn körs visas en ikon i meddelandefältet i Aktivitetsfältet.</span><span class="sxs-lookup"><span data-stu-id="b5f87-157">When the emulator is running, you'll see an icon in the Windows taskbar notification area.</span></span> ![Azure DB Cosmos lokala emulatorn Aktivitetsfältsmeddelande](./media/local-emulator/database-local-emulator-taskbar.png)

<span data-ttu-id="b5f87-159">Azure Cosmos-DB-emulatorn som standard körs på den lokala datorn (”localhost”) som lyssnar på port 8081.</span><span class="sxs-lookup"><span data-stu-id="b5f87-159">The Azure Cosmos DB Emulator by default runs on the local machine ("localhost") listening on port 8081.</span></span>

<span data-ttu-id="b5f87-160">Azure-emulatorn Cosmos DB installeras som standard till den `C:\Program Files\Azure Cosmos DB Emulator` directory.</span><span class="sxs-lookup"><span data-stu-id="b5f87-160">The Azure Cosmos DB Emulator is installed by default to the `C:\Program Files\Azure Cosmos DB Emulator` directory.</span></span> <span data-ttu-id="b5f87-161">Du kan också starta och stoppa emulator från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b5f87-161">You can also start and stop the emulator from the command-line.</span></span> <span data-ttu-id="b5f87-162">Se [kommandoradsverktyget referens](#command-line) för mer information.</span><span class="sxs-lookup"><span data-stu-id="b5f87-162">See [command-line tool reference](#command-line) for more information.</span></span>

## <a name="start-data-explorer"></a><span data-ttu-id="b5f87-163">Starta Data Explorer</span><span class="sxs-lookup"><span data-stu-id="b5f87-163">Start Data Explorer</span></span>

<span data-ttu-id="b5f87-164">När Azure DB som Cosmos-emulatorn startar öppnas automatiskt Azure Cosmos DB Data Explorer i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="b5f87-164">When the Azure Cosmos DB emulator launches it will automatically open the Azure Cosmos DB Data Explorer in your browser.</span></span> <span data-ttu-id="b5f87-165">Adressen visas som [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span><span class="sxs-lookup"><span data-stu-id="b5f87-165">The address will appear as [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html).</span></span> <span data-ttu-id="b5f87-166">Om du stänger Utforskaren och vill öppna den igen senare, kan du öppna URL: en i webbläsaren eller starta från Azure Cosmos-DB-emulatorn i Windows-ikon som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b5f87-166">If you close the explorer and would like to re-open it later, you can either open the URL in your browser or launch it from the Azure Cosmos DB Emulator in the Windows Tray Icon as shown below.</span></span>

![Azure DB Cosmos lokala emulatorn data explorer programstart](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a><span data-ttu-id="b5f87-168">Söker efter uppdateringar</span><span class="sxs-lookup"><span data-stu-id="b5f87-168">Checking for updates</span></span>
<span data-ttu-id="b5f87-169">Data Explorer anger om det finns en ny uppdatering tillgänglig för hämtning.</span><span class="sxs-lookup"><span data-stu-id="b5f87-169">Data Explorer indicates if there is a new update available for download.</span></span> 

> [!NOTE]
> <span data-ttu-id="b5f87-170">Data som har skapats i en version av Azure Cosmos DB-emulatorn är inte säkert att vara tillgänglig när du använder en annan version.</span><span class="sxs-lookup"><span data-stu-id="b5f87-170">Data created in one version of the Azure Cosmos DB Emulator is not guaranteed to be accessible when using a different version.</span></span> <span data-ttu-id="b5f87-171">Om du behöver spara dina data på lång sikt rekommenderas att du lagrar data i ett Azure DB som Cosmos-konto i stället för Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-171">If you need to persist your data for the long term, it is recommended that you store that data in an Azure Cosmos DB account, rather than in the Azure Cosmos DB Emulator.</span></span> 

## <a name="authenticating-requests"></a><span data-ttu-id="b5f87-172">Autentisering av förfrågningar</span><span class="sxs-lookup"><span data-stu-id="b5f87-172">Authenticating requests</span></span>
<span data-ttu-id="b5f87-173">Precis som med Azure Cosmos-DB i molnet, måste varje begäran som du gör mot Azure Cosmos DB-emulatorn autentiseras.</span><span class="sxs-lookup"><span data-stu-id="b5f87-173">Just as with Azure Cosmos DB in the cloud, every request that you make against the Azure Cosmos DB Emulator must be authenticated.</span></span> <span data-ttu-id="b5f87-174">Azure-emulatorn Cosmos DB stöder ett fast konto och en välkänd autentiseringsnyckel för huvudnyckeln för autentisering.</span><span class="sxs-lookup"><span data-stu-id="b5f87-174">The Azure Cosmos DB Emulator supports a single fixed account and a well-known authentication key for master key authentication.</span></span> <span data-ttu-id="b5f87-175">Kontot och nyckeln är de bara autentiseringsuppgifter som tillåts för användning med Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-175">This account and key are the only credentials permitted for use with the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="b5f87-176">De är:</span><span class="sxs-lookup"><span data-stu-id="b5f87-176">They are:</span></span>

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> <span data-ttu-id="b5f87-177">Huvudnyckeln stöds av Azure Cosmos DB-emulatorn är avsedd att användas endast med emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-177">The master key supported by the Azure Cosmos DB Emulator is intended for use only with the emulator.</span></span> <span data-ttu-id="b5f87-178">Du kan inte använda din produktion Azure Cosmos DB konto och nyckel med Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-178">You cannot use your production Azure Cosmos DB account and key with the Azure Cosmos DB Emulator.</span></span> 

> [!NOTE] 
> <span data-ttu-id="b5f87-179">Om du har startat emulatorn med alternativet/Key kan sedan använda den genererade nyckeln i stället för ”C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw ==”</span><span class="sxs-lookup"><span data-stu-id="b5f87-179">If you have started the emulator with the /Key option, then use the generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="b5f87-180">Dessutom precis som tjänsten Azure Cosmos DB Azure Cosmos DB-emulatorn stöder endast säker kommunikation via SSL.</span><span class="sxs-lookup"><span data-stu-id="b5f87-180">Additionally, just as the Azure Cosmos DB service, the Azure Cosmos DB Emulator supports only secure communication via SSL.</span></span>

## <a name="running-the-emulator-on-a-local-network"></a><span data-ttu-id="b5f87-181">Kör emulatorn i ett lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="b5f87-181">Running the emulator on a local network</span></span>

<span data-ttu-id="b5f87-182">Du kan köra emulatorn i ett lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b5f87-182">You can run the emulator on a local network.</span></span> <span data-ttu-id="b5f87-183">Om du vill aktivera nätverksåtkomst, anger du alternativet /AllowNetworkAccess på den [kommandoraden](#command-line-syntax), vilket kräver att du anger/Key = key_string eller/KeyFile = filnamn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-183">To enable network access, specify the /AllowNetworkAccess option at the [command line](#command-line-syntax), which also requires that you specify /Key=key_string or /KeyFile=file_name.</span></span> <span data-ttu-id="b5f87-184">Du kan använda /GenKeyFile = filnamn för att generera en fil med en slumpmässig nyckel förskott.</span><span class="sxs-lookup"><span data-stu-id="b5f87-184">You can use /GenKeyFile=file_name to generate a file with a random key upfront.</span></span>  <span data-ttu-id="b5f87-185">Sedan kan du skicka att att/KeyFile = filnamn eller/Key = contents_of_file.</span><span class="sxs-lookup"><span data-stu-id="b5f87-185">Then you can pass that to /KeyFile=file_name or /Key=contents_of_file.</span></span>

<span data-ttu-id="b5f87-186">För att aktivera nätverksåtkomst för första gången användaren stänga emulatorn och ta bort den emulator datakatalog (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span><span class="sxs-lookup"><span data-stu-id="b5f87-186">To enable network access for the first time the user should shutdown the emulator and delete the emulator’s data directory (C:\Users\user_name\AppData\Local\CosmosDBEmulator).</span></span>

## <a name="developing-with-the-emulator"></a><span data-ttu-id="b5f87-187">Utveckla med emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-187">Developing with the Emulator</span></span>
<span data-ttu-id="b5f87-188">När du har Azure Cosmos DB emulatorn körs på datorn kan du använda någon stöds [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) eller [Azure Cosmos DB REST API](/rest/api/documentdb/) att interagera med emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-188">Once you have the Azure Cosmos DB Emulator running on your desktop, you can use any supported [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) or the [Azure Cosmos DB REST API](/rest/api/documentdb/) to interact with the Emulator.</span></span> <span data-ttu-id="b5f87-189">Azure-emulatorn Cosmos DB innehåller också en inbyggd Data Explorer där du kan skapa samlingar för DocumentDB och MongoDB APIs och visa och redigera dokument utan att skriva någon kod.</span><span class="sxs-lookup"><span data-stu-id="b5f87-189">The Azure Cosmos DB Emulator also includes a built-in Data Explorer that lets you create collections for the DocumentDB and MongoDB APIs, and view and edit documents without writing any code.</span></span>   

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

<span data-ttu-id="b5f87-190">Om du använder [Azure Cosmos DB Protokollstöd för MongoDB](mongodb-introduction.md), Använd följande anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="b5f87-190">If you're using [Azure Cosmos DB protocol support for MongoDB](mongodb-introduction.md), please use the following connection string:</span></span>

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

<span data-ttu-id="b5f87-191">Du kan använda befintliga verktyg [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) att ansluta till Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-191">You can use existing tools like [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) to connect to the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="b5f87-192">Du kan också migrera data mellan Azure Cosmos DB-emulatorn och tjänsten Azure Cosmos DB använder den [Azure Cosmos DB Datamigreringsverktyg](https://github.com/azure/azure-documentdb-datamigrationtool).</span><span class="sxs-lookup"><span data-stu-id="b5f87-192">You can also migrate data between the Azure Cosmos DB Emulator and the Azure Cosmos DB service using the [Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).</span></span>

> [!NOTE] 
> <span data-ttu-id="b5f87-193">Om du har startat emulatorn med alternativet/Key kan sedan använda den genererade nyckeln i stället för ”C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw ==”</span><span class="sxs-lookup"><span data-stu-id="b5f87-193">If you have started the emulator with the /Key option, then use the generated key instead of "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="</span></span>

<span data-ttu-id="b5f87-194">Med hjälp av Azure DB som Cosmos-emulatorn som standard, kan du skapa upp till 25 enskilda partitionssamlingar eller 1 partitionerad samling.</span><span class="sxs-lookup"><span data-stu-id="b5f87-194">Using the Azure Cosmos DB emulator, by default, you can create up to 25 single partition collections or 1 partitioned collection.</span></span> <span data-ttu-id="b5f87-195">Mer information om hur du ändrar det här värdet finns [PartitionCount värdet](#set-partitioncount).</span><span class="sxs-lookup"><span data-stu-id="b5f87-195">For more information about changing this value, see [Setting the PartitionCount value](#set-partitioncount).</span></span>

## <a name="export-the-ssl-certificate"></a><span data-ttu-id="b5f87-196">Exportera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="b5f87-196">Export the SSL certificate</span></span>

<span data-ttu-id="b5f87-197">.NET-språk och körning kan du använda Windows certifikatarkiv på ett säkert sätt ansluta till den lokala Azure DB som Cosmos-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-197">.NET languages and runtime use the Windows Certificate Store to securely connect to the Azure Cosmos DB local emulator.</span></span> <span data-ttu-id="b5f87-198">Andra språk har sin egen metod för att hantera och använda certifikat.</span><span class="sxs-lookup"><span data-stu-id="b5f87-198">Other languages have their own method of managing and using certificates.</span></span> <span data-ttu-id="b5f87-199">Java använder sin egen [certifikatarkiv](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) medan Python använder [socket omslutningar](https://docs.python.org/2/library/ssl.html).</span><span class="sxs-lookup"><span data-stu-id="b5f87-199">Java uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) whereas Python uses [socket wrappers](https://docs.python.org/2/library/ssl.html).</span></span>

<span data-ttu-id="b5f87-200">För att få ett certifikat som ska användas med språk och körningar som inte integreras med Windows certifikatarkiv behöver du exportera den med hjälp av Windows Certificate Manager.</span><span class="sxs-lookup"><span data-stu-id="b5f87-200">In order to obtain a certificate to use with languages and runtimes that do not integrate with the Windows Certificate Store you will need to export it using the Windows Certificate Manager.</span></span> <span data-ttu-id="b5f87-201">Du kan starta den genom att köra certlm.msc eller Följ steg-för-steg-instruktioner i [exportera Azure Cosmos DB emulatorn certifikat](./local-emulator-export-ssl-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="b5f87-201">You can start it by running certlm.msc or follow the step by step instructions in [Export the Azure Cosmos DB Emulator Certificates](./local-emulator-export-ssl-certificates.md).</span></span> <span data-ttu-id="b5f87-202">När Certifikathanteraren körs kan öppna personliga certifikat som visas nedan och exportera certifikatet med namnet ”DocumentDBEmulatorCertificate” som en Base64-kodad X.509 (.cer)-fil.</span><span class="sxs-lookup"><span data-stu-id="b5f87-202">Once the certificate manager is running, open the Personal Certificates as shown below and export the certificate with the friendly name "DocumentDBEmulatorCertificate" as a BASE-64 encoded X.509 (.cer) file.</span></span>

![Azure DB Cosmos lokala emulatorn SSL-certifikat](./media/local-emulator/database-local-emulator-ssl_certificate.png)

<span data-ttu-id="b5f87-204">X.509-certifikat kan importeras till certifikatarkivet Java genom att följa instruktionerna i [att lägga till ett certifikat i Java Certifikatutfärdarens certifikatarkivet](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span><span class="sxs-lookup"><span data-stu-id="b5f87-204">The X.509 certificate can be imported into the Java certificate store by following the instructions in [Adding a Certificate to the Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store).</span></span> <span data-ttu-id="b5f87-205">När du har importerat certifikatet till certifikatarkivet kommer Java och MongoDB program att kunna ansluta till Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-205">Once the certificate is imported into the certificate store, Java and MongoDB applications will be able to connect to the Azure Cosmos DB Emulator.</span></span>

<span data-ttu-id="b5f87-206">När du ansluter till emulatorn från Python och Node.js SDK, är SSL-kontroll inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="b5f87-206">When connecting to the emulator from Python and Node.js SDKs, SSL verification is disabled.</span></span>

## <span data-ttu-id="b5f87-207"><a id="command-line"></a>Kommandoradsverktyget referens</span><span class="sxs-lookup"><span data-stu-id="b5f87-207"><a id="command-line"></a>Command-line tool reference</span></span>
<span data-ttu-id="b5f87-208">Installationsplatsen, kan du använda kommandoraden att starta och stoppa emulatorn, konfigurera alternativ och utföra andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b5f87-208">From the installation location, you can use the command-line to start and stop the emulator, configure options, and perform other operations.</span></span>

### <a name="command-line-syntax"></a><span data-ttu-id="b5f87-209">Kommandoradssyntaxen</span><span class="sxs-lookup"><span data-stu-id="b5f87-209">Command-line Syntax</span></span>

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

<span data-ttu-id="b5f87-210">Om du vill visa listan över alternativ skriver `CosmosDB.Emulator.exe /?` i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="b5f87-210">To view the list of options, type `CosmosDB.Emulator.exe /?` at the command prompt.</span></span>

<table>
<tr>
  <td><span data-ttu-id="b5f87-211"><strong>Alternativet</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-211"><strong>Option</strong></span></span></td>
  <td><span data-ttu-id="b5f87-212"><strong>Beskrivning</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-212"><strong>Description</strong></span></span></td>
  <td><span data-ttu-id="b5f87-213"><strong>Kommandot</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-213"><strong>Command</strong></span></span></td>
  <td><span data-ttu-id="b5f87-214"><strong>Argument</strong></span><span class="sxs-lookup"><span data-stu-id="b5f87-214"><strong>Arguments</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-215">[Inga argument]</span><span class="sxs-lookup"><span data-stu-id="b5f87-215">[No arguments]</span></span></td>
  <td><span data-ttu-id="b5f87-216">Startar Azure Cosmos-DB-emulatorn med standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="b5f87-216">Starts up the Azure Cosmos DB Emulator with default settings.</span></span></td>
  <td><span data-ttu-id="b5f87-217">CosmosDB.Emulator.exe</span><span class="sxs-lookup"><span data-stu-id="b5f87-217">CosmosDB.Emulator.exe</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-218">[Hjälp]</span><span class="sxs-lookup"><span data-stu-id="b5f87-218">[Help]</span></span></td>
  <td><span data-ttu-id="b5f87-219">Visar lista med kommandoradsargument som stöds.</span><span class="sxs-lookup"><span data-stu-id="b5f87-219">Displays the list of supported command-line arguments.</span></span></td>
  <td><span data-ttu-id="b5f87-220">CosmosDB.Emulator.exe /?</span><span class="sxs-lookup"><span data-stu-id="b5f87-220">CosmosDB.Emulator.exe /?</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-221">avstängning</span><span class="sxs-lookup"><span data-stu-id="b5f87-221">Shutdown</span></span></td>
  <td><span data-ttu-id="b5f87-222">Stänger ned Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-222">Shuts down the Azure Cosmos DB Emulator.</span></span></td>
  <td><span data-ttu-id="b5f87-223">CosmosDB.Emulator.exe Shutdown</span><span class="sxs-lookup"><span data-stu-id="b5f87-223">CosmosDB.Emulator.exe /Shutdown</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-224">DataPath</span><span class="sxs-lookup"><span data-stu-id="b5f87-224">DataPath</span></span></td>
  <td><span data-ttu-id="b5f87-225">Anger den sökväg där du kan lagra filer.</span><span class="sxs-lookup"><span data-stu-id="b5f87-225">Specifies the path in which to store data files.</span></span> <span data-ttu-id="b5f87-226">Standardvärdet är % LocalAppdata%\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="b5f87-226">Default is %LocalAppdata%\CosmosDBEmulator.</span></span></td>
  <td><span data-ttu-id="b5f87-227">CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-227">CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-228">&lt;DataPath&gt;: en tillgänglig sökväg</span><span class="sxs-lookup"><span data-stu-id="b5f87-228">&lt;datapath&gt;: An accessible path</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-229">Port</span><span class="sxs-lookup"><span data-stu-id="b5f87-229">Port</span></span></td>
  <td><span data-ttu-id="b5f87-230">Anger det portnummer som ska användas för emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-230">Specifies the port number to use for the emulator.</span></span>  <span data-ttu-id="b5f87-231">Standardvärdet är 8081.</span><span class="sxs-lookup"><span data-stu-id="b5f87-231">Default is 8081.</span></span></td>
  <td><span data-ttu-id="b5f87-232">CosmosDB.Emulator.exe/port =&lt;port&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-232">CosmosDB.Emulator.exe /Port=&lt;port&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-233">&lt;port&gt;: enskilda portnummer</span><span class="sxs-lookup"><span data-stu-id="b5f87-233">&lt;port&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-234">MongoPort</span><span class="sxs-lookup"><span data-stu-id="b5f87-234">MongoPort</span></span></td>
  <td><span data-ttu-id="b5f87-235">Anger det portnummer som ska användas för MongoDB kompatibilitet API.</span><span class="sxs-lookup"><span data-stu-id="b5f87-235">Specifies the port number to use for MongoDB compatibility API.</span></span> <span data-ttu-id="b5f87-236">Standardvärdet är 10255.</span><span class="sxs-lookup"><span data-stu-id="b5f87-236">Default is 10255.</span></span></td>
  <td><span data-ttu-id="b5f87-237">CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-237">CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-238">&lt;mongoport&gt;: enskilda portnummer</span><span class="sxs-lookup"><span data-stu-id="b5f87-238">&lt;mongoport&gt;: Single port number</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-239">DirectPorts</span><span class="sxs-lookup"><span data-stu-id="b5f87-239">DirectPorts</span></span></td>
  <td><span data-ttu-id="b5f87-240">Anger portarna som ska användas för direkt anslutning.</span><span class="sxs-lookup"><span data-stu-id="b5f87-240">Specifies the ports to use for direct connectivity.</span></span> <span data-ttu-id="b5f87-241">Standardvärdena är 10251,10252,10253,10254.</span><span class="sxs-lookup"><span data-stu-id="b5f87-241">Defaults are 10251,10252,10253,10254.</span></span></td>
  <td><span data-ttu-id="b5f87-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-242">CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-243">&lt;directports&gt;: kommaavgränsad lista över 4 portar</span><span class="sxs-lookup"><span data-stu-id="b5f87-243">&lt;directports&gt;: Comma-delimited list of 4 ports</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-244">Nyckel</span><span class="sxs-lookup"><span data-stu-id="b5f87-244">Key</span></span></td>
  <td><span data-ttu-id="b5f87-245">Auktoriseringsnyckeln för emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-245">Authorization key for the emulator.</span></span> <span data-ttu-id="b5f87-246">Nyckeln måste vara Base64-kodning av en vector 64 byte.</span><span class="sxs-lookup"><span data-stu-id="b5f87-246">Key must be the base-64 encoding of a 64-byte vector.</span></span></td>
  <td><span data-ttu-id="b5f87-247">CosmosDB.Emulator.exe/Key:&lt;nyckel&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-247">CosmosDB.Emulator.exe /Key:&lt;key&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-248">&lt;nyckeln&gt;: nyckeln måste vara Base64-kodning av en 64-byte-vektor</span><span class="sxs-lookup"><span data-stu-id="b5f87-248">&lt;key&gt;: Key must be the base-64 encoding of a 64-byte vector</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-249">EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="b5f87-249">EnableRateLimiting</span></span></td>
  <td><span data-ttu-id="b5f87-250">Anger räntesatsen begäran att begränsa beteendet är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="b5f87-250">Specifies that request rate limiting behavior is enabled.</span></span></td>
  <td><span data-ttu-id="b5f87-251">CosmosDB.Emulator.exe /EnableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="b5f87-251">CosmosDB.Emulator.exe /EnableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-252">DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="b5f87-252">DisableRateLimiting</span></span></td>
  <td><span data-ttu-id="b5f87-253">Anger räntesatsen begäran att begränsa beteendet är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="b5f87-253">Specifies that request rate limiting behavior is disabled.</span></span></td>
  <td><span data-ttu-id="b5f87-254">CosmosDB.Emulator.exe /DisableRateLimiting</span><span class="sxs-lookup"><span data-stu-id="b5f87-254">CosmosDB.Emulator.exe /DisableRateLimiting</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-255">NoUI</span><span class="sxs-lookup"><span data-stu-id="b5f87-255">NoUI</span></span></td>
  <td><span data-ttu-id="b5f87-256">Visa inte emulatorn-användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="b5f87-256">Do not show the emulator user interface.</span></span></td>
  <td><span data-ttu-id="b5f87-257">CosmosDB.Emulator.exe/noui</span><span class="sxs-lookup"><span data-stu-id="b5f87-257">CosmosDB.Emulator.exe /NoUI</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-258">NoExplorer</span><span class="sxs-lookup"><span data-stu-id="b5f87-258">NoExplorer</span></span></td>
  <td><span data-ttu-id="b5f87-259">Visa inte dokumentutforskaren vid start.</span><span class="sxs-lookup"><span data-stu-id="b5f87-259">Don't show document explorer on startup.</span></span></td>
  <td><span data-ttu-id="b5f87-260">CosmosDB.Emulator.exe /NoExplorer</span><span class="sxs-lookup"><span data-stu-id="b5f87-260">CosmosDB.Emulator.exe /NoExplorer</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-261">PartitionCount</span><span class="sxs-lookup"><span data-stu-id="b5f87-261">PartitionCount</span></span></td>
  <td><span data-ttu-id="b5f87-262">Anger det maximala antalet partitionerade samlingar.</span><span class="sxs-lookup"><span data-stu-id="b5f87-262">Specifies the maximum number of partitioned collections.</span></span> <span data-ttu-id="b5f87-263">Se [ändra antalet samlingar](#set-partitioncount) för mer information.</span><span class="sxs-lookup"><span data-stu-id="b5f87-263">See [Change the number of collections](#set-partitioncount) for more information.</span></span></td>
  <td><span data-ttu-id="b5f87-264">CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-264">CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-265">&lt;partitioncount&gt;: maximalt antal tillåtna enskilda partitionssamlingar.</span><span class="sxs-lookup"><span data-stu-id="b5f87-265">&lt;partitioncount&gt;: Maximum number of allowed single partition collections.</span></span> <span data-ttu-id="b5f87-266">Standardvärdet är 25.</span><span class="sxs-lookup"><span data-stu-id="b5f87-266">Default is 25.</span></span> <span data-ttu-id="b5f87-267">Högsta tillåtna är 250.</span><span class="sxs-lookup"><span data-stu-id="b5f87-267">Maximum allowed is 250.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-268">DefaultPartitionCount</span><span class="sxs-lookup"><span data-stu-id="b5f87-268">DefaultPartitionCount</span></span></td>
  <td><span data-ttu-id="b5f87-269">Anger antalet partitioner för en partitionerad samling standard.</span><span class="sxs-lookup"><span data-stu-id="b5f87-269">Specifies the default number of partitions for a partitioned collection.</span></span></td>
  <td><span data-ttu-id="b5f87-270">CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-270">CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-271">&lt;defaultpartitioncount&gt; standardvärdet är 25.</span><span class="sxs-lookup"><span data-stu-id="b5f87-271">&lt;defaultpartitioncount&gt; Default is 25.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-272">AllowNetworkAccess</span><span class="sxs-lookup"><span data-stu-id="b5f87-272">AllowNetworkAccess</span></span></td>
  <td><span data-ttu-id="b5f87-273">Aktiverar åtkomst till emulatorn över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="b5f87-273">Enables access to the emulator over a network.</span></span> <span data-ttu-id="b5f87-274">Du måste också ange/Key =&lt;key_string&gt; eller/KeyFile =&lt;file_name&gt; att aktivera nätverksåtkomst.</span><span class="sxs-lookup"><span data-stu-id="b5f87-274">You must also pass /Key=&lt;key_string&gt; or /KeyFile=&lt;file_name&gt; to enable network access.</span></span></td>
  <td><span data-ttu-id="b5f87-275">CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-275">CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;</span></span><br><br><span data-ttu-id="b5f87-276">eller</span><span class="sxs-lookup"><span data-stu-id="b5f87-276">or</span></span><br><br><span data-ttu-id="b5f87-277">CosmosDB.Emulator.exe /AllowNetworkAccess/KeyFile =&lt;filnamn&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-277">CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-278">NoFirewall</span><span class="sxs-lookup"><span data-stu-id="b5f87-278">NoFirewall</span></span></td>
  <td><span data-ttu-id="b5f87-279">Justera inte brandväggsregler när /AllowNetworkAccess används.</span><span class="sxs-lookup"><span data-stu-id="b5f87-279">Don't adjust firewall rules when /AllowNetworkAccess is used.</span></span></td>
  <td><span data-ttu-id="b5f87-280">CosmosDB.Emulator.exe /NoFirewall</span><span class="sxs-lookup"><span data-stu-id="b5f87-280">CosmosDB.Emulator.exe /NoFirewall</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-281">GenKeyFile</span><span class="sxs-lookup"><span data-stu-id="b5f87-281">GenKeyFile</span></span></td>
  <td><span data-ttu-id="b5f87-282">Skapa en ny auktoriserings-nyckel och spara till den angivna filen.</span><span class="sxs-lookup"><span data-stu-id="b5f87-282">Generate a new authorization key and save to the specified file.</span></span> <span data-ttu-id="b5f87-283">Genererad nyckel kan användas med alternativen/Key eller/KeyFile.</span><span class="sxs-lookup"><span data-stu-id="b5f87-283">The generated key can be used with the /Key or /KeyFile options.</span></span></td>
  <td><span data-ttu-id="b5f87-284">CosmosDB.Emulator.exe /GenKeyFile =&lt;sökvägen till nyckelfilen&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-284">CosmosDB.Emulator.exe  /GenKeyFile=&lt;path to key file&gt;</span></span></td>
  <td></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-285">Konsekvens</span><span class="sxs-lookup"><span data-stu-id="b5f87-285">Consistency</span></span></td>
  <td><span data-ttu-id="b5f87-286">Ange standardnivån för konsekvens för kontot.</span><span class="sxs-lookup"><span data-stu-id="b5f87-286">Set the default consistency level for the account.</span></span></td>
  <td><span data-ttu-id="b5f87-287">CosmosDB.Emulator.exe /Consistency =&lt;konsekvenskontroll&gt;</span><span class="sxs-lookup"><span data-stu-id="b5f87-287">CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</span></span></td>
  <td><span data-ttu-id="b5f87-288">&lt;konsekvenskontroll&gt;: värdet måste vara något av följande [konsekvensnivåer](consistency-levels.md): Session starka, Eventual eller BoundedStaleness.</span><span class="sxs-lookup"><span data-stu-id="b5f87-288">&lt;consistency&gt;: Value must be one of the following [consistency levels](consistency-levels.md): Session, Strong, Eventual, or BoundedStaleness.</span></span>  <span data-ttu-id="b5f87-289">Standardvärdet är Session.</span><span class="sxs-lookup"><span data-stu-id="b5f87-289">The default value is Session.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b5f87-290">?</span><span class="sxs-lookup"><span data-stu-id="b5f87-290">?</span></span></td>
  <td><span data-ttu-id="b5f87-291">Visa hjälpmeddelande.</span><span class="sxs-lookup"><span data-stu-id="b5f87-291">Show the help message.</span></span></td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-the-azure-cosmos-db-emulator-and-azure-cosmos-db"></a><span data-ttu-id="b5f87-292">Skillnader mellan Azure Cosmos DB-emulatorn och Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b5f87-292">Differences between the Azure Cosmos DB Emulator and Azure Cosmos DB</span></span> 
<span data-ttu-id="b5f87-293">Eftersom Azure Cosmos DB-emulatorn är en emulerade miljö som körs på en lokal developer-arbetsstation, finns det vissa skillnader i funktionalitet mellan emulatorn och ett Azure DB som Cosmos-konto i molnet:</span><span class="sxs-lookup"><span data-stu-id="b5f87-293">Because the Azure Cosmos DB Emulator provides an emulated environment running on a local developer workstation, there are some differences in functionality between the emulator and an Azure Cosmos DB account in the cloud:</span></span>

* <span data-ttu-id="b5f87-294">Azure-emulatorn Cosmos DB stöder bara ett fast konto och en välkänd huvudnyckel.</span><span class="sxs-lookup"><span data-stu-id="b5f87-294">The Azure Cosmos DB Emulator supports only a single fixed account and a well-known master key.</span></span>  <span data-ttu-id="b5f87-295">Det går inte att nyckeln återskapas i Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-295">Key regeneration is not possible in the Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="b5f87-296">Azure Cosmos DB-emulatorn är inte en skalbar tjänst och stöder inte ett stort antal samlingar.</span><span class="sxs-lookup"><span data-stu-id="b5f87-296">The Azure Cosmos DB Emulator is not a scalable service and will not support a large number of collections.</span></span>
* <span data-ttu-id="b5f87-297">Azure Cosmos DB-emulatorn inte simulera olika [Azure Cosmos DB konsekvensnivåer](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="b5f87-297">The Azure Cosmos DB Emulator does not simulate different [Azure Cosmos DB consistency levels](consistency-levels.md).</span></span>
* <span data-ttu-id="b5f87-298">Azure Cosmos DB-emulatorn inte simulera [flera regioner replikering](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="b5f87-298">The Azure Cosmos DB Emulator does not simulate [multi-region replication](distribute-data-globally.md).</span></span>
* <span data-ttu-id="b5f87-299">Azure Cosmos DB-emulatorn har inte stöd för tjänsten kvoten åsidosättningar som är tillgängliga i Azure DB som Cosmos-tjänsten (t.ex. dokument storleksgränser, ökad partitionerad samling lagring).</span><span class="sxs-lookup"><span data-stu-id="b5f87-299">The Azure Cosmos DB Emulator does not support the service quota overrides that are available in the Azure Cosmos DB service (e.g. document size limits, increased partitioned collection storage).</span></span>
* <span data-ttu-id="b5f87-300">Som ditt exemplar av Azure Cosmos DB-emulatorn är inte eventuellt uppdaterad med de senaste ändringarna med tjänsten Azure Cosmos DB, ta [Azure Cosmos DB kapacitetsplaneringsverktyg](https://www.documentdb.com/capacityplanner) för att beräkna produktionsbehov genomströmning (RUs) för din programmet.</span><span class="sxs-lookup"><span data-stu-id="b5f87-300">As your copy of the Azure Cosmos DB Emulator might not be up to date with the most recent changes with the Azure Cosmos DB service, please [Azure Cosmos DB capacity planner](https://www.documentdb.com/capacityplanner) to accurately estimate production throughput (RUs) needs of your application.</span></span>

## <span data-ttu-id="b5f87-301"><a id="set-partitioncount"></a>Ändra antalet samlingar</span><span class="sxs-lookup"><span data-stu-id="b5f87-301"><a id="set-partitioncount"></a>Change the number of collections</span></span>

<span data-ttu-id="b5f87-302">Du kan skapa upp till 25 enskilda partitionssamlingar eller 1 partitionerad samling med hjälp av Azure Cosmos DB emulatorn som standard.</span><span class="sxs-lookup"><span data-stu-id="b5f87-302">By default, you can create up to 25 single partition collections, or 1 partitioned collection using the Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="b5f87-303">Genom att ändra den **PartitionCount** värde, som du kan skapa upp till 250 enskilda partitionssamlingar eller 10 partitionerade samlingar eller en kombination av båda som inte överstiger 250 enstaka partitioner (där 1 partitionerad samling = 25 enskild partition samling).</span><span class="sxs-lookup"><span data-stu-id="b5f87-303">By modifying the **PartitionCount** value, you can create up to 250 single partition collections or 10 partitioned collections, or any combination of the two that does not exceed 250 single partitions (where 1 partitioned collection = 25 single partition collection).</span></span>

<span data-ttu-id="b5f87-304">Om du försöker skapa en samling när det aktuella antalet partitioner har överskridits utlöser emulatorn ett undantag för ServiceUnavailable, med följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="b5f87-304">If you attempt to create a collection after the current partition count has been exceeded, the emulator throws a ServiceUnavailable exception, with the following message.</span></span>

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    to bring more and more capacity online, and encourage you to try again. 
    Please do not hesitate to email docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

<span data-ttu-id="b5f87-305">Om du vill ändra antalet samlingar som är tillgängliga att Azure Cosmos DB-emulatorn gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b5f87-305">To change the number of collections available to the Azure Cosmos DB Emulator, do the following:</span></span>

1. <span data-ttu-id="b5f87-306">Ta bort alla lokala data i Azure Cosmos DB-emulatorn genom att högerklicka på den **Azure Cosmos DB emulatorn** ikon i meddelandefältet och sedan klicka **återställa Data...** .</span><span class="sxs-lookup"><span data-stu-id="b5f87-306">Delete all local Azure Cosmos DB Emulator data by right-clicking the **Azure Cosmos DB Emulator** icon on the system tray, and then clicking **Reset Data…**.</span></span>
2. <span data-ttu-id="b5f87-307">Ta bort alla emulatorn data i den här mappen C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span><span class="sxs-lookup"><span data-stu-id="b5f87-307">Delete all emulator data in this folder C:\Users\user_name\AppData\Local\CosmosDBEmulator.</span></span>
3. <span data-ttu-id="b5f87-308">Avsluta alla öppna instanser genom att högerklicka på den **Azure Cosmos DB emulatorn** ikon i meddelandefältet och sedan klicka **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="b5f87-308">Exit all open instances by right-clicking the **Azure Cosmos DB Emulator** icon on the system tray, and then clicking **Exit**.</span></span> <span data-ttu-id="b5f87-309">Det kan ta ett tag för alla instanser att avsluta.</span><span class="sxs-lookup"><span data-stu-id="b5f87-309">It may take a minute for all instances to exit.</span></span>
4. <span data-ttu-id="b5f87-310">Installera den senaste versionen av den [Azure Cosmos DB emulatorn](https://aka.ms/cosmosdb-emulator).</span><span class="sxs-lookup"><span data-stu-id="b5f87-310">Install the latest version of the [Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator).</span></span>
5. <span data-ttu-id="b5f87-311">Starta emulatorn med flaggan PartitionCount genom att ange ett värde < = 250.</span><span class="sxs-lookup"><span data-stu-id="b5f87-311">Launch the emulator with the PartitionCount flag by setting a value <= 250.</span></span> <span data-ttu-id="b5f87-312">Till exempel: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span><span class="sxs-lookup"><span data-stu-id="b5f87-312">For example: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b5f87-313">Felsökning</span><span class="sxs-lookup"><span data-stu-id="b5f87-313">Troubleshooting</span></span>

<span data-ttu-id="b5f87-314">Använd följande tips för att felsöka problem som kan uppstå med Azure DB som Cosmos-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="b5f87-314">Use the following tips to help troubleshoot issues you encounter with the Azure Cosmos DB emulator:</span></span>

- <span data-ttu-id="b5f87-315">Om du har installerat en ny version av emulatorn och fel har uppstått, se till att du återställer data.</span><span class="sxs-lookup"><span data-stu-id="b5f87-315">If you installed a new version of the Emulator and are experiencing errors, ensure you reset your data.</span></span> <span data-ttu-id="b5f87-316">Du kan återställa dina data genom att högerklicka på ikonen Azure Cosmos DB emulatorn i systemfältet och sedan klicka på Återställ Data...</span><span class="sxs-lookup"><span data-stu-id="b5f87-316">You can reset your data by right-clicking the Azure Cosmos DB Emulator icon on the system tray, and then clicking Reset Data….</span></span> <span data-ttu-id="b5f87-317">Om detta inte löser felen, kan du avinstallera och installera appen.</span><span class="sxs-lookup"><span data-stu-id="b5f87-317">If that does not fix the errors, you can uninstall and reinstall the app.</span></span> <span data-ttu-id="b5f87-318">Se [avinstallera lokala emulatorn](#uninstall) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="b5f87-318">See [Uninstall the local emulator](#uninstall) for instructions.</span></span>

- <span data-ttu-id="b5f87-319">Om Azure DB som Cosmos-emulatorn kraschar samla in filer med felsökningsdumpar från c:\Users\user_name\AppData\Local\CrashDumps mappen komprimeras och kopplar dem till ett e-postmeddelande till [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b5f87-319">If the Azure Cosmos DB emulator crashes, collect dump files from c:\Users\user_name\AppData\Local\CrashDumps folder, compress them, and attach them to an email to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="b5f87-320">Om det uppstår krascher i CosmosDB.StartupEntryPoint.exe, kör du följande kommando från en kommandotolk för administratörer:`lodctr /R`</span><span class="sxs-lookup"><span data-stu-id="b5f87-320">If you experience crashes in CosmosDB.StartupEntryPoint.exe, run the following command from an admin command prompt: `lodctr /R`</span></span> 

- <span data-ttu-id="b5f87-321">Om du stöter på ett anslutningsproblem, [samla in spårningsfiler](#trace-files), komprimeras och koppla dem till ett e-postmeddelande till [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b5f87-321">If you encounter a connectivity issue, [collect trace files](#trace-files), compress them, and attach them to an email to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

- <span data-ttu-id="b5f87-322">Om du får en **tjänsten är inte tillgänglig** meddelandet emulatorn kan inte initiera nätverksstacken.</span><span class="sxs-lookup"><span data-stu-id="b5f87-322">If you receive a **Service Unavailable** message, the emulator might be failing to initialize the network stack.</span></span> <span data-ttu-id="b5f87-323">Kontrollera om du har Pulse secure klienten eller Juniper nätverk klienten är installerad, som filterdrivrutiner nätverket kan orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="b5f87-323">Check to see if you have the Pulse secure client or Juniper networks client installed, as their network filter drivers may cause the problem.</span></span> <span data-ttu-id="b5f87-324">Avinstallera drivrutiner från tredje part nätverket filter vanligtvis åtgärdar problemet.</span><span class="sxs-lookup"><span data-stu-id="b5f87-324">Uninstalling third party network filter drivers typically fixes the issue.</span></span>

### <span data-ttu-id="b5f87-325"><a id="trace-files"></a>Samla in spårningsfiler</span><span class="sxs-lookup"><span data-stu-id="b5f87-325"><a id="trace-files"></a>Collect trace files</span></span>

<span data-ttu-id="b5f87-326">Om du vill samla in felsökning spårningar, kör du följande kommandon från en administrativ kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="b5f87-326">To collect debugging traces, run the following commands from an administrative command prompt:</span></span>

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. <span data-ttu-id="b5f87-327">`CosmosDB.Emulator.exe /shutdown`.</span><span class="sxs-lookup"><span data-stu-id="b5f87-327">`CosmosDB.Emulator.exe /shutdown`.</span></span> <span data-ttu-id="b5f87-328">Titta på systemfältet att kontrollera att programmet har avslutats kan det ta ett tag.</span><span class="sxs-lookup"><span data-stu-id="b5f87-328">Watch the system tray to make sure the program has shut down, it may take a minute.</span></span> <span data-ttu-id="b5f87-329">Du kan även klicka **avsluta** i användargränssnittet för Azure DB som Cosmos-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b5f87-329">You can also just click **Exit** in the Azure Cosmos DB emulator user interface.</span></span>
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. <span data-ttu-id="b5f87-330">Återskapa problemet.</span><span class="sxs-lookup"><span data-stu-id="b5f87-330">Reproduce the problem.</span></span> <span data-ttu-id="b5f87-331">Om Data Explorer inte fungerar, behöver du bara vänta tills webbläsaren att öppna under några sekunder att upptäcka fel.</span><span class="sxs-lookup"><span data-stu-id="b5f87-331">If Data Explorer is not working, you only need to wait for the browser to open for a few seconds to catch the error.</span></span>
5. `CosmosDB.Emulator.exe /stoptraces`
6. <span data-ttu-id="b5f87-332">Gå till `%ProgramFiles%\Azure Cosmos DB Emulator` och hitta docdbemulator_000001.etl-filen.</span><span class="sxs-lookup"><span data-stu-id="b5f87-332">Navigate to `%ProgramFiles%\Azure Cosmos DB Emulator` and find the docdbemulator_000001.etl file.</span></span>
7. <span data-ttu-id="b5f87-333">Etl-filen tillsammans med reproducera steg för att skicka [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) för felsökning.</span><span class="sxs-lookup"><span data-stu-id="b5f87-333">Send the .etl file along with repro steps to [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) for debugging.</span></span>

### <span data-ttu-id="b5f87-334"><a id="uninstall"></a>Avinstallera den lokala emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-334"><a id="uninstall"></a>Uninstall the local Emulator</span></span>

1. <span data-ttu-id="b5f87-335">Avsluta alla öppna instanser av den lokala emulatorn genom att högerklicka på ikonen för Azure Cosmos DB emulatorn i systemfältet och sedan klicka på Avsluta.</span><span class="sxs-lookup"><span data-stu-id="b5f87-335">Exit all open instances of the local Emulator by right-clicking the Azure Cosmos DB Emulator icon on the system tray, and then clicking Exit.</span></span> <span data-ttu-id="b5f87-336">Det kan ta ett tag för alla instanser att avsluta.</span><span class="sxs-lookup"><span data-stu-id="b5f87-336">It may take a minute for all instances to exit.</span></span>
2. <span data-ttu-id="b5f87-337">Skriv i sökrutan Windows **appar och funktioner** och klicka på den **appar och funktioner (systeminställningar)** resultat.</span><span class="sxs-lookup"><span data-stu-id="b5f87-337">In the Windows search box, type **Apps & features** and click on the **Apps & features (System settings)** result.</span></span>
3. <span data-ttu-id="b5f87-338">Bläddra till i listan över appar **Azure Cosmos DB emulatorn**, markerar du den, klickar du på **avinstallera**, bekräfta och klickar på **avinstallera** igen.</span><span class="sxs-lookup"><span data-stu-id="b5f87-338">In the list of apps, scroll to **Azure Cosmos DB Emulator**, select it, click **Uninstall**, then confirm and click **Uninstall** again.</span></span>
4. <span data-ttu-id="b5f87-339">När appen avinstalleras, navigera till C:\Users\<användare > \AppData\Local\CosmosDBEmulator och ta bort mappen.</span><span class="sxs-lookup"><span data-stu-id="b5f87-339">When the app is uninstalled, navigate to C:\Users\<user>\AppData\Local\CosmosDBEmulator and delete the folder.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b5f87-340">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5f87-340">Next steps</span></span>

<span data-ttu-id="b5f87-341">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="b5f87-341">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5f87-342">Installerat lokala emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-342">Installed the local Emulator</span></span>
> * <span data-ttu-id="b5f87-343">SLUMP Emulator på Docker för Windows</span><span class="sxs-lookup"><span data-stu-id="b5f87-343">Rand the Emulator on Docker for Windows</span></span>
> * <span data-ttu-id="b5f87-344">Autentiserade begäranden</span><span class="sxs-lookup"><span data-stu-id="b5f87-344">Authenticated requests</span></span>
> * <span data-ttu-id="b5f87-345">Används av Data Explorer i emulatorn</span><span class="sxs-lookup"><span data-stu-id="b5f87-345">Used the Data Explorer in the Emulator</span></span>
> * <span data-ttu-id="b5f87-346">Exporterade SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="b5f87-346">Exported SSL certificates</span></span>
> * <span data-ttu-id="b5f87-347">Kallas emulatorn från kommandoraden</span><span class="sxs-lookup"><span data-stu-id="b5f87-347">Called the Emulator from the command line</span></span>
> * <span data-ttu-id="b5f87-348">Insamlade spårningsfiler</span><span class="sxs-lookup"><span data-stu-id="b5f87-348">Collected trace files</span></span>

<span data-ttu-id="b5f87-349">Du har lärt dig hur du använder lokala emulatorn för kostnadsfria lokal utveckling i de här självstudierna.</span><span class="sxs-lookup"><span data-stu-id="b5f87-349">In this tutorial, you've learned how to use the local Emulator for free local development.</span></span> <span data-ttu-id="b5f87-350">Du kan nu gå vidare till nästa kurs och lär dig hur du exporterar emulatorn SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="b5f87-350">You can now proceed to the next tutorial and learn how to export Emulator SSL certificates.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b5f87-351">Exportera Azure Cosmos DB emulatorn certifikat</span><span class="sxs-lookup"><span data-stu-id="b5f87-351">Export the Azure Cosmos DB Emulator certificates</span></span>](local-emulator-export-ssl-certificates.md)
