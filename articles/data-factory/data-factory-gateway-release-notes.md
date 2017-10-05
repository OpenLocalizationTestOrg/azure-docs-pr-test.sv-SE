---
title: "Viktig information för Data Management Gateway | Microsoft Docs"
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
ms.openlocfilehash: c052d7e9f757164429ce867201b96305e405dce9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="release-notes-for-data-management-gateway"></a><span data-ttu-id="56e50-103">Viktig information för gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="56e50-103">Release notes for Data Management Gateway</span></span>
<span data-ttu-id="56e50-104">En av utmaningarna för moderna dataintegrering är att flytta data till och från lokalt till molnet.</span><span class="sxs-lookup"><span data-stu-id="56e50-104">One of the challenges for modern data integration is to move data to and from on-premises to cloud.</span></span> <span data-ttu-id="56e50-105">Data Factory gör den här integreringen med Data Management Gateway, vilket är en agent som att du kan installera lokalt för att aktivera hybrid dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="56e50-105">Data Factory makes this integration with Data Management Gateway, which is an agent that you can install on-premises to enable hybrid data movement.</span></span>

<span data-ttu-id="56e50-106">Se följande artiklar för detaljerad information om Data Management Gateway och hur du använder den:</span><span class="sxs-lookup"><span data-stu-id="56e50-106">See the following articles for detailed information about Data Management Gateway and how to use it:</span></span>

*  [<span data-ttu-id="56e50-107">Gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="56e50-107">Data Management Gateway</span></span>](data-factory-data-management-gateway.md)
*  [<span data-ttu-id="56e50-108">Flytta data mellan lokala och molnet med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-108">Move data between on-premises and cloud using Azure Data Factory</span></span>](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a><span data-ttu-id="56e50-109">AKTUELL VERSION (2.10.6347.7)</span><span class="sxs-lookup"><span data-stu-id="56e50-109">CURRENT VERSION (2.10.6347.7)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="56e50-110">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="56e50-110">Enhancements-</span></span>
- <span data-ttu-id="56e50-111">Du kan lägga till DNS-poster godkända service bus i stället för vitlistning av alla Azure-IP-adresser från din brandvägg (vid behov).</span><span class="sxs-lookup"><span data-stu-id="56e50-111">You can add DNS entries to whitelist service bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="56e50-112">Du hittar motsvarande DNS-post på Azure-portalen (Data Factory -> 'Författare och distribution' -> 'Gateways' -> ”serviceUrls” (i JSON)</span><span class="sxs-lookup"><span data-stu-id="56e50-112">You can find respective DNS entry on Azure portal (Data Factory -> ‘Author and Deploy’ -> ‘Gateways’ -> "serviceUrls" (in JSON)</span></span>
- <span data-ttu-id="56e50-113">HDFS connector stöder nu självsignerat certifikat med offentlig genom att du kan hoppa över validering av SSL.</span><span class="sxs-lookup"><span data-stu-id="56e50-113">HDFS connector now supports self-signed public certificate by letting you skip SSL validation.</span></span>
- <span data-ttu-id="56e50-114">Fast: Problem med gateway offline under uppdateringen (på grund av klockavvikelser)</span><span class="sxs-lookup"><span data-stu-id="56e50-114">Fixed: Issue with gateway offline during update (due to clock skew)</span></span>



## <a name="earlier-versions"></a><span data-ttu-id="56e50-115">Tidigare versioner</span><span class="sxs-lookup"><span data-stu-id="56e50-115">Earlier versions</span></span>

## <a name="2963132"></a><span data-ttu-id="56e50-116">2.9.6313.2</span><span class="sxs-lookup"><span data-stu-id="56e50-116">2.9.6313.2</span></span>
### <a name="enhancements-"></a><span data-ttu-id="56e50-117">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="56e50-117">Enhancements-</span></span>
-   <span data-ttu-id="56e50-118">Du kan lägga till DNS-poster godkända Service Bus i stället för vitlistning av alla Azure-IP-adresser från din brandvägg (vid behov).</span><span class="sxs-lookup"><span data-stu-id="56e50-118">You can add DNS entries to whitelist Service Bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="56e50-119">Mer information här.</span><span class="sxs-lookup"><span data-stu-id="56e50-119">More details here.</span></span>
-   <span data-ttu-id="56e50-120">Nu kan du kopieringsdata till och från ett enda block blob upp till 4,75 TB, vilket är den största storleken som stöds av blockblob.</span><span class="sxs-lookup"><span data-stu-id="56e50-120">You can now copy data to/from a single block blob up to 4.75 TB, which is the max supported size of block blob.</span></span> <span data-ttu-id="56e50-121">(tidigare gränsen var 195 GB).</span><span class="sxs-lookup"><span data-stu-id="56e50-121">(earlier limit was 195 GB).</span></span>
-   <span data-ttu-id="56e50-122">Fast: Slut på minne problemet när man öppnar flera mindre filer under kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="56e50-122">Fixed: Out of memory issue while unzipping several small files during copy activity.</span></span>
-   <span data-ttu-id="56e50-123">Fast: Indexet ligger utanför intervallet problem vid kopiering från dokumentet DB till en lokal SQL Server med idempotens funktionen.</span><span class="sxs-lookup"><span data-stu-id="56e50-123">Fixed: Index out of range issue while copying from Document DB to an on-premises SQL Server with idempotency feature.</span></span>
-   <span data-ttu-id="56e50-124">Fast: SQL-skriptet för rensning av fungerar inte med lokala SQL Server från guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="56e50-124">Fixed: SQL cleanup script doesn't work with on-premises SQL Server from Copy Wizard.</span></span>
-   <span data-ttu-id="56e50-125">Fast: Kolumnnamnet med utrymme i slutet fungerar inte i en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="56e50-125">Fixed: Column name with space at the end does not work in copy activity.</span></span>

## <a name="28662833"></a><span data-ttu-id="56e50-126">2.8.66283.3</span><span class="sxs-lookup"><span data-stu-id="56e50-126">2.8.66283.3</span></span>
### <a name="enhancements-"></a><span data-ttu-id="56e50-127">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="56e50-127">Enhancements-</span></span>
- <span data-ttu-id="56e50-128">Fast: Problem med saknas autentiseringsuppgifter för gateway-datorn startas om.</span><span class="sxs-lookup"><span data-stu-id="56e50-128">Fixed: Issue with missing credentials on gateway machine reboot.</span></span>
- <span data-ttu-id="56e50-129">Fast: Problem med registreringen under gateway återställa en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="56e50-129">Fixed: Issue with registration during gateway restore using a backup file.</span></span>


## <a name="2762401"></a><span data-ttu-id="56e50-130">2.7.6240.1</span><span class="sxs-lookup"><span data-stu-id="56e50-130">2.7.6240.1</span></span>
### <a name="enhancements-"></a><span data-ttu-id="56e50-131">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="56e50-131">Enhancements-</span></span>
- <span data-ttu-id="56e50-132">Fast: Felaktig läsning av null decimalvärde från Oracle som källa.</span><span class="sxs-lookup"><span data-stu-id="56e50-132">Fixed: Incorrect read of Decimal null value from Oracle as source.</span></span>

## <a name="2661922"></a><span data-ttu-id="56e50-133">2.6.6192.2</span><span class="sxs-lookup"><span data-stu-id="56e50-133">2.6.6192.2</span></span>
### <a name="whats-new"></a><span data-ttu-id="56e50-134">Nyheter</span><span class="sxs-lookup"><span data-stu-id="56e50-134">What’s new</span></span>
- <span data-ttu-id="56e50-135">Kunder kan ge feedback på gateway registrerar upplevelse.</span><span class="sxs-lookup"><span data-stu-id="56e50-135">Customers can provide feedback on gateway registering experience.</span></span>
- <span data-ttu-id="56e50-136">Stöd för ett nytt komprimeringsformat för: ZIP (Deflate)</span><span class="sxs-lookup"><span data-stu-id="56e50-136">Support a new compression format: ZIP (Deflate)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="56e50-137">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="56e50-137">Enhancements-</span></span>
- <span data-ttu-id="56e50-138">Förbättring av prestanda för Oracle Sink HDFS källa.</span><span class="sxs-lookup"><span data-stu-id="56e50-138">Performance improvement for Oracle Sink, HDFS source.</span></span>
- <span data-ttu-id="56e50-139">Buggfix för gateway automatiskt uppdatera gateway parallell bearbetning av kapacitet.</span><span class="sxs-lookup"><span data-stu-id="56e50-139">Bug fix for gateway auto update, gateway parallel processing capacity.</span></span>


## <a name="2561641"></a><span data-ttu-id="56e50-140">2.5.6164.1</span><span class="sxs-lookup"><span data-stu-id="56e50-140">2.5.6164.1</span></span>
### <a name="enhancements"></a><span data-ttu-id="56e50-141">Förbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-141">Enhancements</span></span>
- <span data-ttu-id="56e50-142">Bättre och mer robust Gateway registrering experience – nu kan du spåra förloppsstatus under registreringen Gateway, vilket gör registreringen uppleva snabbare.</span><span class="sxs-lookup"><span data-stu-id="56e50-142">Improved and more robust Gateway registration experience- Now you can track progress status during the Gateway registration process, which makes the registration experience more responsive.</span></span>
- <span data-ttu-id="56e50-143">Förbättring i Gateway återställa Process - du kan fortfarande återställa gateway, även om du inte har den säkerhetskopiera filen gateway med den här uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="56e50-143">Improvement in Gateway Restore Process- You can still recover gateway even if you do not have the gateway backup file with this update.</span></span> <span data-ttu-id="56e50-144">Detta kräver du återställa länkade tjänsten autentiseringsuppgifter i portalen.</span><span class="sxs-lookup"><span data-stu-id="56e50-144">This would require you to reset Linked Service credentials in Portal.</span></span>
- <span data-ttu-id="56e50-145">Buggfix.</span><span class="sxs-lookup"><span data-stu-id="56e50-145">Bug fix.</span></span>

## <a name="2461511"></a><span data-ttu-id="56e50-146">2.4.6151.1</span><span class="sxs-lookup"><span data-stu-id="56e50-146">2.4.6151.1</span></span>

### <a name="whats-new"></a><span data-ttu-id="56e50-147">Nyheter</span><span class="sxs-lookup"><span data-stu-id="56e50-147">What’s new</span></span>

- <span data-ttu-id="56e50-148">Nu kan du lagra autentiseringsuppgifterna för datakällan lokalt.</span><span class="sxs-lookup"><span data-stu-id="56e50-148">You can now store data source credentials locally.</span></span> <span data-ttu-id="56e50-149">Autentiseringsuppgifterna krypteras.</span><span class="sxs-lookup"><span data-stu-id="56e50-149">The credentials are encrypted.</span></span> <span data-ttu-id="56e50-150">Autentiseringsuppgifterna för datakällan kan återställas och återställas med hjälp av säkerhetskopian som kan exporteras från den befintliga gatewayen, alla lokala.</span><span class="sxs-lookup"><span data-stu-id="56e50-150">The data source credentials can be recovered and restored using the backup file that can be exported from the existing Gateway, all on-premises.</span></span>

### <a name="enhancements-"></a><span data-ttu-id="56e50-151">Förbättringar-</span><span class="sxs-lookup"><span data-stu-id="56e50-151">Enhancements-</span></span>

- <span data-ttu-id="56e50-152">Bättre och mer robust Gateway registrering upplevelse.</span><span class="sxs-lookup"><span data-stu-id="56e50-152">Improved and more robust Gateway registration experience.</span></span>
- <span data-ttu-id="56e50-153">Stöd för automatisk identifiering av QuoteChar konfiguration för textformat i guiden Kopiera och förbättra övergripande format identifiering.</span><span class="sxs-lookup"><span data-stu-id="56e50-153">Support auto detection of QuoteChar configuration for Text format in copy wizard, and improve the overall format detection accuracy.</span></span>

## <a name="2361002"></a><span data-ttu-id="56e50-154">2.3.6100.2</span><span class="sxs-lookup"><span data-stu-id="56e50-154">2.3.6100.2</span></span>

- <span data-ttu-id="56e50-155">Stöd för firstRowAsHeader och automatisk identifiering av SkipLineCount i guiden Kopiera textfiler i lokala filsystemet och HDFS.</span><span class="sxs-lookup"><span data-stu-id="56e50-155">Support firstRowAsHeader and SkipLineCount auto detection in copy wizard for text files in on-premises File system and HDFS.</span></span>
- <span data-ttu-id="56e50-156">Förbättra stabiliteten för nätverksanslutningen mellan gateway och Service Bus</span><span class="sxs-lookup"><span data-stu-id="56e50-156">Enhance the stability of network connection between gateway and Service Bus</span></span>
- <span data-ttu-id="56e50-157">Några buggkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-157">A few bug fixes</span></span>


## <a name="2260721"></a><span data-ttu-id="56e50-158">2.2.6072.1</span><span class="sxs-lookup"><span data-stu-id="56e50-158">2.2.6072.1</span></span>

*  <span data-ttu-id="56e50-159">Har stöd för inställningen HTTP-proxy för gatewayen med Konfigurationshanteraren för Gateway.</span><span class="sxs-lookup"><span data-stu-id="56e50-159">Supports setting HTTP proxy for the gateway using the Gateway Configuration Manager.</span></span> <span data-ttu-id="56e50-160">Om konfigurerad, kan Azure Blob, Azure Table, Azure Data Lake och dokumentet DB nås via HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="56e50-160">If configured, Azure Blob, Azure Table, Azure Data Lake, and Document DB are accessed through HTTP proxy.</span></span>
*  <span data-ttu-id="56e50-161">Har stöd för huvudet hantering för TextFormat vid kopiering av data från/till Azure Blob Azure Data Lake Store lokalt filsystem, och lokala HDFS.</span><span class="sxs-lookup"><span data-stu-id="56e50-161">Supports header handling for TextFormat when copying data from/to Azure Blob, Azure Data Lake Store, on-premises File System, and on-premises HDFS.</span></span>
*  <span data-ttu-id="56e50-162">Kopiera data från Lägg till Blob och Sidblob tillsammans med redan stöds Blockblob stöder.</span><span class="sxs-lookup"><span data-stu-id="56e50-162">Supports copying data from Append Blob and Page Blob along with the already supported Block Blob.</span></span>
*  <span data-ttu-id="56e50-163">Introducerar en ny gateway status **Online (begränsat)**, vilket tyder på att de flesta funktioner för gatewayen fungerar utom interaktiva åtgärden stöd för guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="56e50-163">Introduces a new gateway status **Online (Limited)**, which indicates that the main functionality of the gateway works except the interactive operation support for Copy Wizard.</span></span>
*  <span data-ttu-id="56e50-164">Förbättrar gateway-registrering med registreringsnyckel stabilitet.</span><span class="sxs-lookup"><span data-stu-id="56e50-164">Enhances the robustness of gateway registration using registration key.</span></span>

## <a name="216040"></a><span data-ttu-id="56e50-165">2.1.6040.</span><span class="sxs-lookup"><span data-stu-id="56e50-165">2.1.6040.</span></span>

*  <span data-ttu-id="56e50-166">DB2 drivrutin ingår nu i gateway-installationspaketet.</span><span class="sxs-lookup"><span data-stu-id="56e50-166">DB2 driver is included in the gateway installation package now.</span></span> <span data-ttu-id="56e50-167">Du behöver inte installera det separat.</span><span class="sxs-lookup"><span data-stu-id="56e50-167">You do not need to install it separately.</span></span>
*  <span data-ttu-id="56e50-168">DB2-drivrutinen stöder nu z/OS och DB2 för jag (AS / 400) tillsammans med de plattformar som redan stöds (Linux, Unix- och Windows).</span><span class="sxs-lookup"><span data-stu-id="56e50-168">DB2 driver now supports z/OS and DB2 for i (AS/400) along with the platforms already supported (Linux, Unix, and Windows).</span></span>
*  <span data-ttu-id="56e50-169">Stöder Azure Cosmos DB som källa eller mål för lokala datalager</span><span class="sxs-lookup"><span data-stu-id="56e50-169">Supports using Azure Cosmos DB as a source or destination for on-premises data stores</span></span>
*  <span data-ttu-id="56e50-170">Har stöd för kopiering av data från/till kall/frekvent blob-lagring tillsammans med redan stöds Allmänt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="56e50-170">Supports copying data from/to cold/hot blob storage along with the already supported general-purpose storage account.</span></span>
*  <span data-ttu-id="56e50-171">Kan du ansluta till lokal SQL Server via en gateway med behörighet för fjärrinloggning.</span><span class="sxs-lookup"><span data-stu-id="56e50-171">Allows you to connect to on-premises SQL Server via gateway with remote login privileges.</span></span>  

## <a name="2060131"></a><span data-ttu-id="56e50-172">2.0.6013.1</span><span class="sxs-lookup"><span data-stu-id="56e50-172">2.0.6013.1</span></span>

*  <span data-ttu-id="56e50-173">Du kan välja språkkulturen som ska användas av en gateway vid manuell installation.</span><span class="sxs-lookup"><span data-stu-id="56e50-173">You can select the language/culture to be used by a gateway during manual installation.</span></span>

*  <span data-ttu-id="56e50-174">När gatewayen inte fungerar som förväntat, kan du välja att skicka gateway-loggarna för senaste sju dagarna till Microsoft för att underlätta felsökning av problemet.</span><span class="sxs-lookup"><span data-stu-id="56e50-174">When gateway does not work as expected, you can choose to send gateway logs of last seven days to Microsoft to facilitate troubleshooting of the issue.</span></span> <span data-ttu-id="56e50-175">Om gateway inte är ansluten till Molntjänsten kan du välja att spara och arkivera gateway-loggarna.</span><span class="sxs-lookup"><span data-stu-id="56e50-175">If gateway is not connected to the cloud service, you can choose to save and archive gateway logs.</span></span>  

*  <span data-ttu-id="56e50-176">Användargränssnittet för gateway configuration manager:</span><span class="sxs-lookup"><span data-stu-id="56e50-176">User interface improvements for gateway configuration manager:</span></span>

    *  <span data-ttu-id="56e50-177">Se gatewayens status tydligare på fliken Start.</span><span class="sxs-lookup"><span data-stu-id="56e50-177">Make gateway status more visible on the Home tab.</span></span>

    *  <span data-ttu-id="56e50-178">Organiseras och förenklad kontroller.</span><span class="sxs-lookup"><span data-stu-id="56e50-178">Reorganized and simplified controls.</span></span>

    *  <span data-ttu-id="56e50-179">Du kan kopiera data från en lagring med hjälp av den [kod utan kopiera preview verktyget](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="56e50-179">You can copy data from a storage using the [code-free copy preview tool](data-factory-copy-data-wizard-tutorial.md).</span></span> <span data-ttu-id="56e50-180">Se [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) mer information om den här funktionen i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="56e50-180">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details about this feature in general.</span></span>
*  <span data-ttu-id="56e50-181">Du kan använda Data Management Gateway till ingång data direkt från en lokal SQL Server-databas i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="56e50-181">You can use Data Management Gateway to ingress data directly from an on-premises SQL Server database into Azure Machine Learning.</span></span>

*  <span data-ttu-id="56e50-182">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-182">Performance improvements</span></span>

    * <span data-ttu-id="56e50-183">Förbättra prestanda i Visa Schema/Förhandsgranska mot SQL Server i kod-fri kopiering preview-verktyget.</span><span class="sxs-lookup"><span data-stu-id="56e50-183">Improve performance on viewing Schema/Preview against SQL Server in code-free copy preview tool.</span></span>

## <a name="11259531"></a><span data-ttu-id="56e50-184">1.12.5953.1</span><span class="sxs-lookup"><span data-stu-id="56e50-184">1.12.5953.1</span></span>

*  <span data-ttu-id="56e50-185">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-185">Bug fixes</span></span>

## <a name="11159181"></a><span data-ttu-id="56e50-186">1.11.5918.1</span><span class="sxs-lookup"><span data-stu-id="56e50-186">1.11.5918.1</span></span>

*  <span data-ttu-id="56e50-187">Maximal storlek för gateway-händelseloggen har ökat från 1 MB till 40 MB.</span><span class="sxs-lookup"><span data-stu-id="56e50-187">Maximum size of the gateway event log has been increased from 1 MB to 40 MB.</span></span>

*  <span data-ttu-id="56e50-188">Ett varningsmeddelande visas om en omstart krävs under gateway automatisk uppdatering.</span><span class="sxs-lookup"><span data-stu-id="56e50-188">A warning dialog is displayed in case a restart is needed during gateway auto-update.</span></span> <span data-ttu-id="56e50-189">Du kan välja att starta om normalt sedan eller senare.</span><span class="sxs-lookup"><span data-stu-id="56e50-189">You can choose to restart right then or later.</span></span>

*  <span data-ttu-id="56e50-190">Om automatisk uppdatering misslyckas, försöker installationsprogrammet för gateway automatisk uppdatering tre gånger på maximalt.</span><span class="sxs-lookup"><span data-stu-id="56e50-190">In case auto-update fails, gateway installer retries auto-updating three times at maximum.</span></span>

*  <span data-ttu-id="56e50-191">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-191">Performance improvements</span></span>

    * <span data-ttu-id="56e50-192">Förbättra prestanda för inläsning av stora tabeller från lokala servern i kod-fri kopiering scenario.</span><span class="sxs-lookup"><span data-stu-id="56e50-192">Improve performance for loading large tables from on-premises server in code-free copy scenario.</span></span>

*  <span data-ttu-id="56e50-193">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-193">Bug fixes</span></span>

## <a name="11058921"></a><span data-ttu-id="56e50-194">1.10.5892.1</span><span class="sxs-lookup"><span data-stu-id="56e50-194">1.10.5892.1</span></span>

*  <span data-ttu-id="56e50-195">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-195">Performance improvements</span></span>

*  <span data-ttu-id="56e50-196">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-196">Bug fixes</span></span>

## <a name="1958652"></a><span data-ttu-id="56e50-197">1.9.5865.2</span><span class="sxs-lookup"><span data-stu-id="56e50-197">1.9.5865.2</span></span>

*  <span data-ttu-id="56e50-198">Funktionen noll touch automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="56e50-198">Zero touch auto update capability</span></span>
*  <span data-ttu-id="56e50-199">Ny ikon med indikatorer för gateway-status</span><span class="sxs-lookup"><span data-stu-id="56e50-199">New tray icon with gateway status indicators</span></span>
*  <span data-ttu-id="56e50-200">Möjligheten att ”uppdatera nu” från klienten</span><span class="sxs-lookup"><span data-stu-id="56e50-200">Ability to “Update now” from the client</span></span>
*  <span data-ttu-id="56e50-201">Möjlighet att ställa in schemat Uppdateringstid</span><span class="sxs-lookup"><span data-stu-id="56e50-201">Ability to set update schedule time</span></span>
*  <span data-ttu-id="56e50-202">PowerShell-skript för att använda automatisk uppdatering av/på</span><span class="sxs-lookup"><span data-stu-id="56e50-202">PowerShell script for toggling auto-update on/off</span></span>
*  <span data-ttu-id="56e50-203">Stöd för JSON-format</span><span class="sxs-lookup"><span data-stu-id="56e50-203">Support for JSON format</span></span>  
*  <span data-ttu-id="56e50-204">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-204">Performance improvements</span></span>
*  <span data-ttu-id="56e50-205">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-205">Bug fixes</span></span>

## <a name="1858221"></a><span data-ttu-id="56e50-206">1.8.5822.1</span><span class="sxs-lookup"><span data-stu-id="56e50-206">1.8.5822.1</span></span>

*  <span data-ttu-id="56e50-207">Förbättra felsökning</span><span class="sxs-lookup"><span data-stu-id="56e50-207">Improve troubleshooting experience</span></span>
*  <span data-ttu-id="56e50-208">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-208">Performance improvements</span></span>
*  <span data-ttu-id="56e50-209">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-209">Bug fixes</span></span>

### <a name="1757951"></a><span data-ttu-id="56e50-210">1.7.5795.1</span><span class="sxs-lookup"><span data-stu-id="56e50-210">1.7.5795.1</span></span>

*  <span data-ttu-id="56e50-211">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-211">Performance improvements</span></span>
*  <span data-ttu-id="56e50-212">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-212">Bug fixes</span></span>

### <a name="1757641"></a><span data-ttu-id="56e50-213">1.7.5764.1</span><span class="sxs-lookup"><span data-stu-id="56e50-213">1.7.5764.1</span></span>

*  <span data-ttu-id="56e50-214">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-214">Performance improvements</span></span>
*  <span data-ttu-id="56e50-215">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-215">Bug fixes</span></span>

### <a name="1657351"></a><span data-ttu-id="56e50-216">1.6.5735.1</span><span class="sxs-lookup"><span data-stu-id="56e50-216">1.6.5735.1</span></span>

*  <span data-ttu-id="56e50-217">Stöd för lokala HDFS källor/mottagare</span><span class="sxs-lookup"><span data-stu-id="56e50-217">Support on-premises HDFS Source/Sink</span></span>
*  <span data-ttu-id="56e50-218">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-218">Performance improvements</span></span>
*  <span data-ttu-id="56e50-219">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-219">Bug fixes</span></span>

### <a name="1656961"></a><span data-ttu-id="56e50-220">1.6.5696.1</span><span class="sxs-lookup"><span data-stu-id="56e50-220">1.6.5696.1</span></span>

*  <span data-ttu-id="56e50-221">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-221">Performance improvements</span></span>
*  <span data-ttu-id="56e50-222">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-222">Bug fixes</span></span>

### <a name="1656761"></a><span data-ttu-id="56e50-223">1.6.5676.1</span><span class="sxs-lookup"><span data-stu-id="56e50-223">1.6.5676.1</span></span>

*  <span data-ttu-id="56e50-224">Diagnostiska supportverktyg på Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="56e50-224">Support diagnostic tools on Configuration Manager</span></span>
*  <span data-ttu-id="56e50-225">Stöd för tabellkolumnerna tabelldata källor för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-225">Support table columns for tabular data sources for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-226">SQL DW-stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-226">Support SQL DW for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-227">Stöd för Reclusive i BlobSource och FileSource för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-227">Support Reclusive in BlobSource and FileSource for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-228">Stöd för CopyBehavior – MergeFiles, PreserveHierarchy och FlattenHierarchy i BlobSink och FileSink med binära kopia för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-228">Support CopyBehavior – MergeFiles, PreserveHierarchy, and FlattenHierarchy in BlobSink and FileSink with Binary Copy for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-229">Stöd för Kopieringsaktiviteten statusrapportering för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-229">Support Copy Activity reporting progress for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-230">Stöd för källa anslutningen dataverifiering för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-230">Support Data Source Connectivity Validation for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-231">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-231">Bug fixes</span></span>

### <a name="1656721"></a><span data-ttu-id="56e50-232">1.6.5672.1</span><span class="sxs-lookup"><span data-stu-id="56e50-232">1.6.5672.1</span></span>

*  <span data-ttu-id="56e50-233">Tabellnamnet för ODBC-datakällan har stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-233">Support table name for ODBC data source for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-234">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-234">Performance improvements</span></span>
*  <span data-ttu-id="56e50-235">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-235">Bug fixes</span></span>

### <a name="1656581"></a><span data-ttu-id="56e50-236">1.6.5658.1</span><span class="sxs-lookup"><span data-stu-id="56e50-236">1.6.5658.1</span></span>

*  <span data-ttu-id="56e50-237">Stöd för filen Sink för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-237">Support File Sink for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-238">Stöd för bevara hierarki i binär kopia för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-238">Support preserving hierarchy in binary copy for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-239">Kopiera aktivitet idempotens har stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-239">Support Copy Activity Idempotency for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-240">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-240">Bug fixes</span></span>

### <a name="1656401"></a><span data-ttu-id="56e50-241">1.6.5640.1</span><span class="sxs-lookup"><span data-stu-id="56e50-241">1.6.5640.1</span></span>

*  <span data-ttu-id="56e50-242">Stöd för 3 fler datakällor för Azure Data Factory (ODBC, OData, HDFS)</span><span class="sxs-lookup"><span data-stu-id="56e50-242">Support 3 more data sources for Azure Data Factory (ODBC, OData, HDFS)</span></span>
*  <span data-ttu-id="56e50-243">Citattecken i csv-parser stöd för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-243">Support quote character in csv parser for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-244">Komprimering support (BZip2)</span><span class="sxs-lookup"><span data-stu-id="56e50-244">Compression support (BZip2)</span></span>
*  <span data-ttu-id="56e50-245">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-245">Bug fixes</span></span>

### <a name="1556121"></a><span data-ttu-id="56e50-246">1.5.5612.1</span><span class="sxs-lookup"><span data-stu-id="56e50-246">1.5.5612.1</span></span>

*  <span data-ttu-id="56e50-247">Stöd för fem relationsdatabaser för Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata och Sybase)</span><span class="sxs-lookup"><span data-stu-id="56e50-247">Support five relational databases for Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata, and Sybase)</span></span>
*  <span data-ttu-id="56e50-248">Komprimering support (Gzip och Deflate)</span><span class="sxs-lookup"><span data-stu-id="56e50-248">Compression support (Gzip and Deflate)</span></span>
*  <span data-ttu-id="56e50-249">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-249">Performance improvements</span></span>
*  <span data-ttu-id="56e50-250">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-250">Bug fixes</span></span>

### <a name="1455491"></a><span data-ttu-id="56e50-251">1.4.5549.1</span><span class="sxs-lookup"><span data-stu-id="56e50-251">1.4.5549.1</span></span>

*  <span data-ttu-id="56e50-252">Lägga till stöd för Oracle datakällan för Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56e50-252">Add Oracle data source support for Azure Data Factory</span></span>
*  <span data-ttu-id="56e50-253">Prestandaförbättringar</span><span class="sxs-lookup"><span data-stu-id="56e50-253">Performance improvements</span></span>
*  <span data-ttu-id="56e50-254">Felkorrigeringar</span><span class="sxs-lookup"><span data-stu-id="56e50-254">Bug fixes</span></span>

### <a name="1454921"></a><span data-ttu-id="56e50-255">1.4.5492.1</span><span class="sxs-lookup"><span data-stu-id="56e50-255">1.4.5492.1</span></span>

*  <span data-ttu-id="56e50-256">Enhetlig binary som har stöd för både Microsoft Azure Data Factory och Power BI för Office 365-tjänster</span><span class="sxs-lookup"><span data-stu-id="56e50-256">Unified binary that supports both Microsoft Azure Data Factory and Office 365 Power BI services</span></span>
*  <span data-ttu-id="56e50-257">Förfina processen användargränssnitt och registrering</span><span class="sxs-lookup"><span data-stu-id="56e50-257">Refine the Configuration UI and registration process</span></span>
*  <span data-ttu-id="56e50-258">Azure Data Factory – Azure ingående och utgående stöd för SQL Server-datakälla</span><span class="sxs-lookup"><span data-stu-id="56e50-258">Azure Data Factory – Azure Ingress and Egress support for SQL Server data source</span></span>

### <a name="1253031"></a><span data-ttu-id="56e50-259">1.2.5303.1</span><span class="sxs-lookup"><span data-stu-id="56e50-259">1.2.5303.1</span></span>

*  <span data-ttu-id="56e50-260">Åtgärda problemet tidsgränsen för att stödja mer tidskrävande datakällanslutningar.</span><span class="sxs-lookup"><span data-stu-id="56e50-260">Fix timeout issue to support more time-consuming data source connections.</span></span>

### <a name="1155268"></a><span data-ttu-id="56e50-261">1.1.5526.8</span><span class="sxs-lookup"><span data-stu-id="56e50-261">1.1.5526.8</span></span>

*  <span data-ttu-id="56e50-262">Kräver .NET Framework 4.5.1 som en förutsättning under installationen.</span><span class="sxs-lookup"><span data-stu-id="56e50-262">Requires .NET Framework 4.5.1 as a prerequisite during setup.</span></span>

### <a name="1051442"></a><span data-ttu-id="56e50-263">1.0.5144.2</span><span class="sxs-lookup"><span data-stu-id="56e50-263">1.0.5144.2</span></span>

*  <span data-ttu-id="56e50-264">Inga ändringar som påverkar Azure Data Factory-scenarier.</span><span class="sxs-lookup"><span data-stu-id="56e50-264">No changes that affect Azure Data Factory scenarios.</span></span>
