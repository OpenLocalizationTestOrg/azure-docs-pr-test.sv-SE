---
title: "aaaRelease anteckningar för Data Management Gateway | Microsoft Docs"
description: Data Management Gateway tory viktig information
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 3165d7537410a0531e0bb7f7fe584767f9155574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-data-management-gateway"></a><span data-ttu-id="645b3-103">Viktig information för gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="645b3-103">Release notes for Data Management Gateway</span></span>
<span data-ttu-id="645b3-104">En av hello utmaningar för moderna dataintegrering är toomove data tooand från lokala toocloud.</span><span class="sxs-lookup"><span data-stu-id="645b3-104">One of hello challenges for modern data integration is toomove data tooand from on-premises toocloud.</span></span> <span data-ttu-id="645b3-105">Data Factory gör den här integreringen med Data Management Gateway, vilket är en agent som du installerar lokalt tooenable hybrid dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="645b3-105">Data Factory makes this integration with Data Management Gateway, which is an agent that you can install on-premises tooenable hybrid data movement.</span></span>

<span data-ttu-id="645b3-106">Se följande artiklar för detaljerad information om Data Management Gateway hello och hur toouse den:</span><span class="sxs-lookup"><span data-stu-id="645b3-106">See hello following articles for detailed information about Data Management Gateway and how toouse it:</span></span>

*  [<span data-ttu-id="645b3-107">Gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="645b3-107">Data Management Gateway</span></span>](data-factory-data-management-gateway.md)
*  [<span data-ttu-id="645b3-108">Flytta data mellan lokala och molnet med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-108">Move data between on-premises and cloud using Azure Data Factory</span></span>](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a><span data-ttu-id="645b3-109">AKTUELL VERSION (2.10.6347.7)</span><span class="sxs-lookup"><span data-stu-id="645b3-109">CURRENT VERSION (2.10.6347.7)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="645b3-110">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="645b3-110">Enhancements-</span></span>
- <span data-ttu-id="645b3-111">Du kan lägga till DNS-poster toowhitelist service bus i stället för vitlistning av alla Azure-IP-adresser från din brandvägg (vid behov).</span><span class="sxs-lookup"><span data-stu-id="645b3-111">You can add DNS entries toowhitelist service bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="645b3-112">Du hittar motsvarande DNS-post på Azure-portalen (Data Factory -> 'Författare och distribution' -> 'Gateways' -> ”serviceUrls” (i JSON)</span><span class="sxs-lookup"><span data-stu-id="645b3-112">You can find respective DNS entry on Azure portal (Data Factory -> ‘Author and Deploy’ -> ‘Gateways’ -> "serviceUrls" (in JSON)</span></span>
- <span data-ttu-id="645b3-113">HDFS connector stöder nu självsignerat certifikat med offentlig genom att du kan hoppa över validering av SSL.</span><span class="sxs-lookup"><span data-stu-id="645b3-113">HDFS connector now supports self-signed public certificate by letting you skip SSL validation.</span></span>
- <span data-ttu-id="645b3-114">Fast: Problem med gateway offline under uppdateringen (förfaller tooclock förskjutning)</span><span class="sxs-lookup"><span data-stu-id="645b3-114">Fixed: Issue with gateway offline during update (due tooclock skew)</span></span>



## <a name="earlier-versions"></a><span data-ttu-id="645b3-115">Tidigare versioner</span><span class="sxs-lookup"><span data-stu-id="645b3-115">Earlier versions</span></span>

## <a name="2963132"></a><span data-ttu-id="645b3-116">2.9.6313.2</span><span class="sxs-lookup"><span data-stu-id="645b3-116">2.9.6313.2</span></span>
### <a name="enhancements-"></a><span data-ttu-id="645b3-117">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="645b3-117">Enhancements-</span></span>
-   <span data-ttu-id="645b3-118">Du kan lägga till DNS-poster toowhitelist Service Bus i stället för vitlistning av alla Azure-IP-adresser från din brandvägg (vid behov).</span><span class="sxs-lookup"><span data-stu-id="645b3-118">You can add DNS entries toowhitelist Service Bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="645b3-119">Mer information här.</span><span class="sxs-lookup"><span data-stu-id="645b3-119">More details here.</span></span>
-   <span data-ttu-id="645b3-120">Nu kan du kopiera data till och från en enda blockblobb in too4.75 TB, vilket är Hej max stöds storleken på blockblob.</span><span class="sxs-lookup"><span data-stu-id="645b3-120">You can now copy data to/from a single block blob up too4.75 TB, which is hello max supported size of block blob.</span></span> <span data-ttu-id="645b3-121">(tidigare gränsen var 195 GB).</span><span class="sxs-lookup"><span data-stu-id="645b3-121">(earlier limit was 195 GB).</span></span>
-   <span data-ttu-id="645b3-122">Fast: Slut på minne problemet när man öppnar flera mindre filer under kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="645b3-122">Fixed: Out of memory issue while unzipping several small files during copy activity.</span></span>
-   <span data-ttu-id="645b3-123">Fast: Indexet ligger utanför intervallet problem vid kopiering från dokumentet DB tooan lokal SQL Server med idempotens funktionen.</span><span class="sxs-lookup"><span data-stu-id="645b3-123">Fixed: Index out of range issue while copying from Document DB tooan on-premises SQL Server with idempotency feature.</span></span>
-   <span data-ttu-id="645b3-124">Fast: SQL-skriptet för rensning av fungerar inte med lokala SQL Server från guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="645b3-124">Fixed: SQL cleanup script doesn't work with on-premises SQL Server from Copy Wizard.</span></span>
-   <span data-ttu-id="645b3-125">Fast: Kolumnnamnet med utrymme i slutet av hello fungerar inte i en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="645b3-125">Fixed: Column name with space at hello end does not work in copy activity.</span></span>

## <a name="28662833"></a><span data-ttu-id="645b3-126">2.8.66283.3</span><span class="sxs-lookup"><span data-stu-id="645b3-126">2.8.66283.3</span></span>
### <a name="enhancements-"></a><span data-ttu-id="645b3-127">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="645b3-127">Enhancements-</span></span>
- <span data-ttu-id="645b3-128">Fast: Problem med saknas autentiseringsuppgifter för gateway-datorn startas om.</span><span class="sxs-lookup"><span data-stu-id="645b3-128">Fixed: Issue with missing credentials on gateway machine reboot.</span></span>
- <span data-ttu-id="645b3-129">Fast: Problem med registreringen under gateway återställa en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="645b3-129">Fixed: Issue with registration during gateway restore using a backup file.</span></span>


## <a name="2762401"></a><span data-ttu-id="645b3-130">2.7.6240.1</span><span class="sxs-lookup"><span data-stu-id="645b3-130">2.7.6240.1</span></span>
### <a name="enhancements-"></a><span data-ttu-id="645b3-131">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="645b3-131">Enhancements-</span></span>
- <span data-ttu-id="645b3-132">Fast: Felaktig läsning av null decimalvärde från Oracle som källa.</span><span class="sxs-lookup"><span data-stu-id="645b3-132">Fixed: Incorrect read of Decimal null value from Oracle as source.</span></span>

## <a name="2661922"></a><span data-ttu-id="645b3-133">2.6.6192.2</span><span class="sxs-lookup"><span data-stu-id="645b3-133">2.6.6192.2</span></span>
### <a name="whats-new"></a><span data-ttu-id="645b3-134">Nyheter</span><span class="sxs-lookup"><span data-stu-id="645b3-134">What’s new</span></span>
- <span data-ttu-id="645b3-135">Kunder kan ge feedback på gateway registrerar upplevelse.</span><span class="sxs-lookup"><span data-stu-id="645b3-135">Customers can provide feedback on gateway registering experience.</span></span>
- <span data-ttu-id="645b3-136">Stöd för ett nytt komprimeringsformat för: ZIP (Deflate)</span><span class="sxs-lookup"><span data-stu-id="645b3-136">Support a new compression format: ZIP (Deflate)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="645b3-137">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="645b3-137">Enhancements-</span></span>
- <span data-ttu-id="645b3-138">Förbättring av prestanda för Oracle Sink HDFS källa.</span><span class="sxs-lookup"><span data-stu-id="645b3-138">Performance improvement for Oracle Sink, HDFS source.</span></span>
- <span data-ttu-id="645b3-139">Buggfix för gateway automatiskt uppdatera gateway parallell bearbetning av kapacitet.</span><span class="sxs-lookup"><span data-stu-id="645b3-139">Bug fix for gateway auto update, gateway parallel processing capacity.</span></span>


## <a name="2561641"></a><span data-ttu-id="645b3-140">2.5.6164.1</span><span class="sxs-lookup"><span data-stu-id="645b3-140">2.5.6164.1</span></span>
### <a name="enhancements"></a><span data-ttu-id="645b3-141">Förbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-141">Enhancements</span></span>
- <span data-ttu-id="645b3-142">Bättre och mer robust Gateway registrering experience – nu kan du spåra förloppsstatus under hello Gateway registreringen, vilket gör hello registrering får snabbare.</span><span class="sxs-lookup"><span data-stu-id="645b3-142">Improved and more robust Gateway registration experience- Now you can track progress status during hello Gateway registration process, which makes hello registration experience more responsive.</span></span>
- <span data-ttu-id="645b3-143">Förbättring i Gateway återställa Process - du kan fortfarande återställa gateway, även om du inte har hello gateway-säkerhetskopia med den här uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="645b3-143">Improvement in Gateway Restore Process- You can still recover gateway even if you do not have hello gateway backup file with this update.</span></span> <span data-ttu-id="645b3-144">Detta skulle kräva tooreset länkade tjänsten autentiseringsuppgifter i portalen.</span><span class="sxs-lookup"><span data-stu-id="645b3-144">This would require you tooreset Linked Service credentials in Portal.</span></span>
- <span data-ttu-id="645b3-145">Buggfix.</span><span class="sxs-lookup"><span data-stu-id="645b3-145">Bug fix.</span></span>

## <a name="2461511"></a><span data-ttu-id="645b3-146">2.4.6151.1</span><span class="sxs-lookup"><span data-stu-id="645b3-146">2.4.6151.1</span></span>

### <a name="whats-new"></a><span data-ttu-id="645b3-147">Nyheter</span><span class="sxs-lookup"><span data-stu-id="645b3-147">What’s new</span></span>

- <span data-ttu-id="645b3-148">Nu kan du lagra autentiseringsuppgifterna för datakällan lokalt.</span><span class="sxs-lookup"><span data-stu-id="645b3-148">You can now store data source credentials locally.</span></span> <span data-ttu-id="645b3-149">hello autentiseringsuppgifterna krypteras.</span><span class="sxs-lookup"><span data-stu-id="645b3-149">hello credentials are encrypted.</span></span> <span data-ttu-id="645b3-150">hello autentiseringsuppgifterna för datakällan kan återställas och återställas med hjälp av hello säkerhetskopian som kan exporteras från hello befintlig Gateway, alla lokala.</span><span class="sxs-lookup"><span data-stu-id="645b3-150">hello data source credentials can be recovered and restored using hello backup file that can be exported from hello existing Gateway, all on-premises.</span></span>

### <a name="enhancements-"></a><span data-ttu-id="645b3-151">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="645b3-151">Enhancements-</span></span>

- <span data-ttu-id="645b3-152">Bättre och mer robust Gateway registrering upplevelse.</span><span class="sxs-lookup"><span data-stu-id="645b3-152">Improved and more robust Gateway registration experience.</span></span>
- <span data-ttu-id="645b3-153">Stöd för automatisk identifiering av QuoteChar konfiguration för textformat i guiden Kopiera och förbättra hello övergripande formatera identifiering noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="645b3-153">Support auto detection of QuoteChar configuration for Text format in copy wizard, and improve hello overall format detection accuracy.</span></span>

## <a name="2361002"></a><span data-ttu-id="645b3-154">2.3.6100.2</span><span class="sxs-lookup"><span data-stu-id="645b3-154">2.3.6100.2</span></span>

- <span data-ttu-id="645b3-155">Stöd för firstRowAsHeader och automatisk identifiering av SkipLineCount i guiden Kopiera textfiler i lokala filsystemet och HDFS.</span><span class="sxs-lookup"><span data-stu-id="645b3-155">Support firstRowAsHeader and SkipLineCount auto detection in copy wizard for text files in on-premises File system and HDFS.</span></span>
- <span data-ttu-id="645b3-156">Förbättra hello stabiliteten för nätverksanslutningen mellan gateway och Service Bus</span><span class="sxs-lookup"><span data-stu-id="645b3-156">Enhance hello stability of network connection between gateway and Service Bus</span></span>
- <span data-ttu-id="645b3-157">Några buggkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-157">A few bug fixes</span></span>


## <a name="2260721"></a><span data-ttu-id="645b3-158">2.2.6072.1</span><span class="sxs-lookup"><span data-stu-id="645b3-158">2.2.6072.1</span></span>

*  <span data-ttu-id="645b3-159">Har stöd för inställningen HTTP-proxy för hello gateway med hello Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="645b3-159">Supports setting HTTP proxy for hello gateway using hello Gateway Configuration Manager.</span></span> <span data-ttu-id="645b3-160">Om konfigurerad, kan Azure Blob, Azure Table, Azure Data Lake och dokumentet DB nås via HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="645b3-160">If configured, Azure Blob, Azure Table, Azure Data Lake, and Document DB are accessed through HTTP proxy.</span></span>
*  <span data-ttu-id="645b3-161">Har stöd för huvudet hantering för TextFormat vid kopiering av data från / tooAzure Blob, Azure Data Lake Store lokala filsystemet och lokala HDFS.</span><span class="sxs-lookup"><span data-stu-id="645b3-161">Supports header handling for TextFormat when copying data from/tooAzure Blob, Azure Data Lake Store, on-premises File System, and on-premises HDFS.</span></span>
*  <span data-ttu-id="645b3-162">Kopiera data från Lägg till Blob och Sidblob tillsammans med hello redan stöder stöds Blockblob.</span><span class="sxs-lookup"><span data-stu-id="645b3-162">Supports copying data from Append Blob and Page Blob along with hello already supported Block Blob.</span></span>
*  <span data-ttu-id="645b3-163">Introducerar en ny gateway status **Online (begränsat)**, vilket anger att hello huvudfunktioner hello gateway fungerar utom hello interaktiva åtgärden stöd för guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="645b3-163">Introduces a new gateway status **Online (Limited)**, which indicates that hello main functionality of hello gateway works except hello interactive operation support for Copy Wizard.</span></span>
*  <span data-ttu-id="645b3-164">Förbättrar hello stabilitet av gateway-registrering med hjälp av nyckel för tjänstregistrering.</span><span class="sxs-lookup"><span data-stu-id="645b3-164">Enhances hello robustness of gateway registration using registration key.</span></span>

## <a name="216040"></a><span data-ttu-id="645b3-165">2.1.6040.</span><span class="sxs-lookup"><span data-stu-id="645b3-165">2.1.6040.</span></span>

*  <span data-ttu-id="645b3-166">DB2 drivrutin ingår nu i hello gateway-installationspaketet.</span><span class="sxs-lookup"><span data-stu-id="645b3-166">DB2 driver is included in hello gateway installation package now.</span></span> <span data-ttu-id="645b3-167">Du behöver inte tooinstall den separat.</span><span class="sxs-lookup"><span data-stu-id="645b3-167">You do not need tooinstall it separately.</span></span>
*  <span data-ttu-id="645b3-168">DB2-drivrutinen stöder nu z/OS och DB2 för jag (AS / 400) tillsammans med hello plattformar som redan stöds (Linux, Unix- och Windows).</span><span class="sxs-lookup"><span data-stu-id="645b3-168">DB2 driver now supports z/OS and DB2 for i (AS/400) along with hello platforms already supported (Linux, Unix, and Windows).</span></span>
*  <span data-ttu-id="645b3-169">Stöder Azure Cosmos DB som källa eller mål för lokala datalager</span><span class="sxs-lookup"><span data-stu-id="645b3-169">Supports using Azure Cosmos DB as a source or destination for on-premises data stores</span></span>
*  <span data-ttu-id="645b3-170">Har stöd för kopiering av data från/toocold/hot blob-lagring tillsammans med hello redan stöds Allmänt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="645b3-170">Supports copying data from/toocold/hot blob storage along with hello already supported general-purpose storage account.</span></span>
*  <span data-ttu-id="645b3-171">Låter dig tooconnect tooon lokala SQL Server via en gateway med behörighet för fjärrinloggning.</span><span class="sxs-lookup"><span data-stu-id="645b3-171">Allows you tooconnect tooon-premises SQL Server via gateway with remote login privileges.</span></span>  

## <a name="2060131"></a><span data-ttu-id="645b3-172">2.0.6013.1</span><span class="sxs-lookup"><span data-stu-id="645b3-172">2.0.6013.1</span></span>

*  <span data-ttu-id="645b3-173">Du kan välja hello språkkulturen/toobe används av en gateway vid manuell installation.</span><span class="sxs-lookup"><span data-stu-id="645b3-173">You can select hello language/culture toobe used by a gateway during manual installation.</span></span>

*  <span data-ttu-id="645b3-174">När gatewayen inte fungerar som förväntat, kan du välja toosend gateway-loggarna för senaste sju dagarna tooMicrosoft toofacilitate felsökning av hello problemet.</span><span class="sxs-lookup"><span data-stu-id="645b3-174">When gateway does not work as expected, you can choose toosend gateway logs of last seven days tooMicrosoft toofacilitate troubleshooting of hello issue.</span></span> <span data-ttu-id="645b3-175">Om gateway inte är ansluten toohello Molntjänsten, kan du välja toosave och arkivera gateway-loggarna.</span><span class="sxs-lookup"><span data-stu-id="645b3-175">If gateway is not connected toohello cloud service, you can choose toosave and archive gateway logs.</span></span>  

*  <span data-ttu-id="645b3-176">Användargränssnittet för gateway configuration manager:</span><span class="sxs-lookup"><span data-stu-id="645b3-176">User interface improvements for gateway configuration manager:</span></span>

    *  <span data-ttu-id="645b3-177">Se gatewayens status tydligare på fliken för hello start.</span><span class="sxs-lookup"><span data-stu-id="645b3-177">Make gateway status more visible on hello Home tab.</span></span>

    *  <span data-ttu-id="645b3-178">Organiseras och förenklad kontroller.</span><span class="sxs-lookup"><span data-stu-id="645b3-178">Reorganized and simplified controls.</span></span>

    *  <span data-ttu-id="645b3-179">Du kan kopiera data från en lagring med hjälp av hello [kod utan kopiera preview verktyget](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="645b3-179">You can copy data from a storage using hello [code-free copy preview tool](data-factory-copy-data-wizard-tutorial.md).</span></span> <span data-ttu-id="645b3-180">Se [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) mer information om den här funktionen i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="645b3-180">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details about this feature in general.</span></span>
*  <span data-ttu-id="645b3-181">Du kan använda Data Management Gateway tooingress data direkt från en lokal SQL Server-databas i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="645b3-181">You can use Data Management Gateway tooingress data directly from an on-premises SQL Server database into Azure Machine Learning.</span></span>

*  <span data-ttu-id="645b3-182">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-182">Performance improvements</span></span>

    * <span data-ttu-id="645b3-183">Förbättra prestanda i Visa Schema/Förhandsgranska mot SQL Server i kod-fri kopiering preview-verktyget.</span><span class="sxs-lookup"><span data-stu-id="645b3-183">Improve performance on viewing Schema/Preview against SQL Server in code-free copy preview tool.</span></span>

## <a name="11259531"></a><span data-ttu-id="645b3-184">1.12.5953.1</span><span class="sxs-lookup"><span data-stu-id="645b3-184">1.12.5953.1</span></span>

*  <span data-ttu-id="645b3-185">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-185">Bug fixes</span></span>

## <a name="11159181"></a><span data-ttu-id="645b3-186">1.11.5918.1</span><span class="sxs-lookup"><span data-stu-id="645b3-186">1.11.5918.1</span></span>

*  <span data-ttu-id="645b3-187">Maximal storlek för händelselogg hello har ökat från 1 MB too40 MB.</span><span class="sxs-lookup"><span data-stu-id="645b3-187">Maximum size of hello gateway event log has been increased from 1 MB too40 MB.</span></span>

*  <span data-ttu-id="645b3-188">Ett varningsmeddelande visas om en omstart krävs under gateway automatisk uppdatering.</span><span class="sxs-lookup"><span data-stu-id="645b3-188">A warning dialog is displayed in case a restart is needed during gateway auto-update.</span></span> <span data-ttu-id="645b3-189">Du kan välja toorestart höger sedan eller senare.</span><span class="sxs-lookup"><span data-stu-id="645b3-189">You can choose toorestart right then or later.</span></span>

*  <span data-ttu-id="645b3-190">Om automatisk uppdatering misslyckas, försöker installationsprogrammet för gateway automatisk uppdatering tre gånger på maximalt.</span><span class="sxs-lookup"><span data-stu-id="645b3-190">In case auto-update fails, gateway installer retries auto-updating three times at maximum.</span></span>

*  <span data-ttu-id="645b3-191">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-191">Performance improvements</span></span>

    * <span data-ttu-id="645b3-192">Förbättra prestanda för inläsning av stora tabeller från lokala servern i kod-fri kopiering scenario.</span><span class="sxs-lookup"><span data-stu-id="645b3-192">Improve performance for loading large tables from on-premises server in code-free copy scenario.</span></span>

*  <span data-ttu-id="645b3-193">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-193">Bug fixes</span></span>

## <a name="11058921"></a><span data-ttu-id="645b3-194">1.10.5892.1</span><span class="sxs-lookup"><span data-stu-id="645b3-194">1.10.5892.1</span></span>

*  <span data-ttu-id="645b3-195">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-195">Performance improvements</span></span>

*  <span data-ttu-id="645b3-196">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-196">Bug fixes</span></span>

## <a name="1958652"></a><span data-ttu-id="645b3-197">1.9.5865.2</span><span class="sxs-lookup"><span data-stu-id="645b3-197">1.9.5865.2</span></span>

*  <span data-ttu-id="645b3-198">Funktionen noll touch automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="645b3-198">Zero touch auto update capability</span></span>
*  <span data-ttu-id="645b3-199">Ny ikon med indikatorer för gateway-status</span><span class="sxs-lookup"><span data-stu-id="645b3-199">New tray icon with gateway status indicators</span></span>
*  <span data-ttu-id="645b3-200">Möjlighet för ”Uppdatera nu” hello-klient</span><span class="sxs-lookup"><span data-stu-id="645b3-200">Ability too“Update now” from hello client</span></span>
*  <span data-ttu-id="645b3-201">Möjlighet tooset Uppdateringstid schema</span><span class="sxs-lookup"><span data-stu-id="645b3-201">Ability tooset update schedule time</span></span>
*  <span data-ttu-id="645b3-202">PowerShell-skript för att använda automatisk uppdatering av/på</span><span class="sxs-lookup"><span data-stu-id="645b3-202">PowerShell script for toggling auto-update on/off</span></span>
*  <span data-ttu-id="645b3-203">Stöd för JSON-format</span><span class="sxs-lookup"><span data-stu-id="645b3-203">Support for JSON format</span></span>  
*  <span data-ttu-id="645b3-204">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-204">Performance improvements</span></span>
*  <span data-ttu-id="645b3-205">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-205">Bug fixes</span></span>

## <a name="1858221"></a><span data-ttu-id="645b3-206">1.8.5822.1</span><span class="sxs-lookup"><span data-stu-id="645b3-206">1.8.5822.1</span></span>

*  <span data-ttu-id="645b3-207">Förbättra felsökning</span><span class="sxs-lookup"><span data-stu-id="645b3-207">Improve troubleshooting experience</span></span>
*  <span data-ttu-id="645b3-208">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-208">Performance improvements</span></span>
*  <span data-ttu-id="645b3-209">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-209">Bug fixes</span></span>

### <a name="1757951"></a><span data-ttu-id="645b3-210">1.7.5795.1</span><span class="sxs-lookup"><span data-stu-id="645b3-210">1.7.5795.1</span></span>

*  <span data-ttu-id="645b3-211">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-211">Performance improvements</span></span>
*  <span data-ttu-id="645b3-212">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-212">Bug fixes</span></span>

### <a name="1757641"></a><span data-ttu-id="645b3-213">1.7.5764.1</span><span class="sxs-lookup"><span data-stu-id="645b3-213">1.7.5764.1</span></span>

*  <span data-ttu-id="645b3-214">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-214">Performance improvements</span></span>
*  <span data-ttu-id="645b3-215">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-215">Bug fixes</span></span>

### <a name="1657351"></a><span data-ttu-id="645b3-216">1.6.5735.1</span><span class="sxs-lookup"><span data-stu-id="645b3-216">1.6.5735.1</span></span>

*  <span data-ttu-id="645b3-217">Stöd för lokala HDFS källor/mottagare</span><span class="sxs-lookup"><span data-stu-id="645b3-217">Support on-premises HDFS Source/Sink</span></span>
*  <span data-ttu-id="645b3-218">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-218">Performance improvements</span></span>
*  <span data-ttu-id="645b3-219">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-219">Bug fixes</span></span>

### <a name="1656961"></a><span data-ttu-id="645b3-220">1.6.5696.1</span><span class="sxs-lookup"><span data-stu-id="645b3-220">1.6.5696.1</span></span>

*  <span data-ttu-id="645b3-221">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-221">Performance improvements</span></span>
*  <span data-ttu-id="645b3-222">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-222">Bug fixes</span></span>

### <a name="1656761"></a><span data-ttu-id="645b3-223">1.6.5676.1</span><span class="sxs-lookup"><span data-stu-id="645b3-223">1.6.5676.1</span></span>

*  <span data-ttu-id="645b3-224">Diagnostiska supportverktyg på Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="645b3-224">Support diagnostic tools on Configuration Manager</span></span>
*  <span data-ttu-id="645b3-225">Stöd för tabellkolumnerna tabelldata källor för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-225">Support table columns for tabular data sources for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-226">SQL DW-stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-226">Support SQL DW for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-227">Stöd för Reclusive i BlobSource och FileSource för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-227">Support Reclusive in BlobSource and FileSource for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-228">Stöd för CopyBehavior – MergeFiles, PreserveHierarchy och FlattenHierarchy i BlobSink och FileSink med binära kopia för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-228">Support CopyBehavior – MergeFiles, PreserveHierarchy, and FlattenHierarchy in BlobSink and FileSink with Binary Copy for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-229">Stöd för Kopieringsaktiviteten statusrapportering för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-229">Support Copy Activity reporting progress for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-230">Stöd för källa anslutningen dataverifiering för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-230">Support Data Source Connectivity Validation for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-231">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-231">Bug fixes</span></span>

### <a name="1656721"></a><span data-ttu-id="645b3-232">1.6.5672.1</span><span class="sxs-lookup"><span data-stu-id="645b3-232">1.6.5672.1</span></span>

*  <span data-ttu-id="645b3-233">Tabellnamnet för ODBC-datakällan har stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-233">Support table name for ODBC data source for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-234">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-234">Performance improvements</span></span>
*  <span data-ttu-id="645b3-235">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-235">Bug fixes</span></span>

### <a name="1656581"></a><span data-ttu-id="645b3-236">1.6.5658.1</span><span class="sxs-lookup"><span data-stu-id="645b3-236">1.6.5658.1</span></span>

*  <span data-ttu-id="645b3-237">Stöd för filen Sink för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-237">Support File Sink for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-238">Stöd för bevara hierarki i binär kopia för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-238">Support preserving hierarchy in binary copy for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-239">Kopiera aktivitet idempotens har stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-239">Support Copy Activity Idempotency for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-240">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-240">Bug fixes</span></span>

### <a name="1656401"></a><span data-ttu-id="645b3-241">1.6.5640.1</span><span class="sxs-lookup"><span data-stu-id="645b3-241">1.6.5640.1</span></span>

*  <span data-ttu-id="645b3-242">Stöd för 3 fler datakällor för Azure Data Factory (ODBC, OData, HDFS)</span><span class="sxs-lookup"><span data-stu-id="645b3-242">Support 3 more data sources for Azure Data Factory (ODBC, OData, HDFS)</span></span>
*  <span data-ttu-id="645b3-243">Citattecken i csv-parser stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-243">Support quote character in csv parser for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-244">Komprimering support (BZip2)</span><span class="sxs-lookup"><span data-stu-id="645b3-244">Compression support (BZip2)</span></span>
*  <span data-ttu-id="645b3-245">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-245">Bug fixes</span></span>

### <a name="1556121"></a><span data-ttu-id="645b3-246">1.5.5612.1</span><span class="sxs-lookup"><span data-stu-id="645b3-246">1.5.5612.1</span></span>

*  <span data-ttu-id="645b3-247">Stöd för fem relationsdatabaser för Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata och Sybase)</span><span class="sxs-lookup"><span data-stu-id="645b3-247">Support five relational databases for Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata, and Sybase)</span></span>
*  <span data-ttu-id="645b3-248">Komprimering support (Gzip och Deflate)</span><span class="sxs-lookup"><span data-stu-id="645b3-248">Compression support (Gzip and Deflate)</span></span>
*  <span data-ttu-id="645b3-249">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-249">Performance improvements</span></span>
*  <span data-ttu-id="645b3-250">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-250">Bug fixes</span></span>

### <a name="1455491"></a><span data-ttu-id="645b3-251">1.4.5549.1</span><span class="sxs-lookup"><span data-stu-id="645b3-251">1.4.5549.1</span></span>

*  <span data-ttu-id="645b3-252">Lägga till stöd för Oracle datakällan för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="645b3-252">Add Oracle data source support for Azure Data Factory</span></span>
*  <span data-ttu-id="645b3-253">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="645b3-253">Performance improvements</span></span>
*  <span data-ttu-id="645b3-254">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="645b3-254">Bug fixes</span></span>

### <a name="1454921"></a><span data-ttu-id="645b3-255">1.4.5492.1</span><span class="sxs-lookup"><span data-stu-id="645b3-255">1.4.5492.1</span></span>

*  <span data-ttu-id="645b3-256">Enhetlig binary som har stöd för både Microsoft Azure Data Factory och Power BI för Office 365-tjänster</span><span class="sxs-lookup"><span data-stu-id="645b3-256">Unified binary that supports both Microsoft Azure Data Factory and Office 365 Power BI services</span></span>
*  <span data-ttu-id="645b3-257">Förfina hello användargränssnitt och registrering processen</span><span class="sxs-lookup"><span data-stu-id="645b3-257">Refine hello Configuration UI and registration process</span></span>
*  <span data-ttu-id="645b3-258">Azure Data Factory – Azure ingående och utgående stöd för SQL Server-datakälla</span><span class="sxs-lookup"><span data-stu-id="645b3-258">Azure Data Factory – Azure Ingress and Egress support for SQL Server data source</span></span>

### <a name="1253031"></a><span data-ttu-id="645b3-259">1.2.5303.1</span><span class="sxs-lookup"><span data-stu-id="645b3-259">1.2.5303.1</span></span>

*  <span data-ttu-id="645b3-260">Åtgärda timeout problemet toosupport mer tidskrävande datakällanslutningar.</span><span class="sxs-lookup"><span data-stu-id="645b3-260">Fix timeout issue toosupport more time-consuming data source connections.</span></span>

### <a name="1155268"></a><span data-ttu-id="645b3-261">1.1.5526.8</span><span class="sxs-lookup"><span data-stu-id="645b3-261">1.1.5526.8</span></span>

*  <span data-ttu-id="645b3-262">Kräver .NET Framework 4.5.1 som en förutsättning under installationen.</span><span class="sxs-lookup"><span data-stu-id="645b3-262">Requires .NET Framework 4.5.1 as a prerequisite during setup.</span></span>

### <a name="1051442"></a><span data-ttu-id="645b3-263">1.0.5144.2</span><span class="sxs-lookup"><span data-stu-id="645b3-263">1.0.5144.2</span></span>

*  <span data-ttu-id="645b3-264">Inga ändringar som påverkar Azure Data Factory-scenarier.</span><span class="sxs-lookup"><span data-stu-id="645b3-264">No changes that affect Azure Data Factory scenarios.</span></span>
