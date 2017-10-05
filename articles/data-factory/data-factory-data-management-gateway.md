---
title: "Data Management Gateway för Data Factory | Microsoft Docs"
description: "Konfigurera en datagateway för att flytta data mellan lokalt och i molnet. Flytta dina data med hjälp av Data Management Gateway i Azure Data Factory."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 9e40eba285aeb1cce6b77311d1b69a6b96967a0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="data-management-gateway"></a><span data-ttu-id="1d950-104">Gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="1d950-104">Data Management Gateway</span></span>
<span data-ttu-id="1d950-105">Data management gateway är en klientagent som du måste installera i din lokala miljö för att kopiera data mellan molnet och lokala datalager.</span><span class="sxs-lookup"><span data-stu-id="1d950-105">The Data management gateway is a client agent that you must install in your on-premises environment to copy data between cloud and on-premises data stores.</span></span> <span data-ttu-id="1d950-106">Lokala data lagras som stöds av Data Factory visas i den [datakällor som stöds i](data-factory-data-movement-activities.md#supported-data-stores-and-formats) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d950-106">The on-premises data stores supported by Data Factory are listed in the [Supported data sources](data-factory-data-movement-activities.md#supported-data-stores-and-formats) section.</span></span>

<span data-ttu-id="1d950-107">Den här artikeln kompletterar genomgången i den [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1d950-107">This article complements the walkthrough in the [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="1d950-108">I den här genomgången kan du skapa en pipeline som använder en gateway för att flytta data från en lokal SQL Server-databas till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="1d950-108">In the walkthrough, you create a pipeline that uses the gateway to move data from an on-premises SQL Server database to an Azure blob.</span></span> <span data-ttu-id="1d950-109">Den här artikeln innehåller detaljerad information om data management gateway.</span><span class="sxs-lookup"><span data-stu-id="1d950-109">This article provides detailed in-depth information about the data management gateway.</span></span> 

<span data-ttu-id="1d950-110">Du kan skala ut en data management gateway genom att associera flera lokala datorer med gatewayen.</span><span class="sxs-lookup"><span data-stu-id="1d950-110">You can scale out a data management gateway by associating multiple on-premises machines with the gateway.</span></span> <span data-ttu-id="1d950-111">Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod.</span><span class="sxs-lookup"><span data-stu-id="1d950-111">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="1d950-112">Den här funktionen finns också en logisk gateway med en enda nod.</span><span class="sxs-lookup"><span data-stu-id="1d950-112">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="1d950-113">Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="1d950-113">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>

> [!NOTE]
> <span data-ttu-id="1d950-114">Gateway stöder för närvarande endast den kopia och lagrade proceduraktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1d950-114">Currently, gateway supports only the copy activity and stored procedure activity in Data Factory.</span></span> <span data-ttu-id="1d950-115">Det går inte att använda gatewayen från en anpassad aktivitet för att få åtkomst till lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="1d950-115">It is not possible to use the gateway from a custom activity to access on-premises data sources.</span></span>      

## <a name="overview"></a><span data-ttu-id="1d950-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="1d950-116">Overview</span></span>
### <a name="capabilities-of-data-management-gateway"></a><span data-ttu-id="1d950-117">Funktioner i data management gateway</span><span class="sxs-lookup"><span data-stu-id="1d950-117">Capabilities of data management gateway</span></span>
<span data-ttu-id="1d950-118">Data management gateway innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="1d950-118">Data management gateway provides the following capabilities:</span></span>

* <span data-ttu-id="1d950-119">Modellen lokalt datakällor och molnet datakällor i samma data factory och flytta data.</span><span class="sxs-lookup"><span data-stu-id="1d950-119">Model on-premises data sources and cloud data sources within the same data factory and move data.</span></span>
* <span data-ttu-id="1d950-120">Ha en och samma plats för övervakning och hantering med insyn i gatewayens status från Data Factory-sidan.</span><span class="sxs-lookup"><span data-stu-id="1d950-120">Have a single pane of glass for monitoring and management with visibility into gateway status from the Data Factory page.</span></span>
* <span data-ttu-id="1d950-121">Hantera åtkomst till lokala datakällor på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="1d950-121">Manage access to on-premises data sources securely.</span></span>
  * <span data-ttu-id="1d950-122">Inga ändringar som krävs för att företagets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="1d950-122">No changes required to corporate firewall.</span></span> <span data-ttu-id="1d950-123">Gateway blir bara utgående HTTP-baserade anslutningar för att öppna internet.</span><span class="sxs-lookup"><span data-stu-id="1d950-123">Gateway only makes outbound HTTP-based connections to open internet.</span></span>
  * <span data-ttu-id="1d950-124">Kryptera autentiseringsuppgifterna för ditt lokala datalager med ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="1d950-124">Encrypt credentials for your on-premises data stores with your certificate.</span></span>
* <span data-ttu-id="1d950-125">Flytta data effektivt – data överförs parallellt, motståndskraftiga mot tillfälliga nätverksproblem med automatiskt försök logik.</span><span class="sxs-lookup"><span data-stu-id="1d950-125">Move data efficiently – data is transferred in parallel, resilient to intermittent network issues with auto retry logic.</span></span>

### <a name="command-flow-and-data-flow"></a><span data-ttu-id="1d950-126">Kommandot trafikflöde och dataflöde</span><span class="sxs-lookup"><span data-stu-id="1d950-126">Command flow and data flow</span></span>
<span data-ttu-id="1d950-127">När du använder en kopieringsaktiviteten för att kopiera data mellan lokalt och i molnet använder aktiviteten en gateway för att överföra data från lokala datakällan till molnet och vice versa.</span><span class="sxs-lookup"><span data-stu-id="1d950-127">When you use a copy activity to copy data between on-premises and cloud, the activity uses a gateway to transfer data from on-premises data source to cloud and vice versa.</span></span>

<span data-ttu-id="1d950-128">Här är den övergripande dataflödet för och översikt över stegen för att kopiera med datagateway: ![dataflöde med gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span><span class="sxs-lookup"><span data-stu-id="1d950-128">Here is the high-level data flow for and summary of steps for copy with data gateway: ![Data flow using gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span></span>

1. <span data-ttu-id="1d950-129">Data utvecklare skapar en gateway för ett Azure Data Factory med antingen den [Azure-portalen](https://portal.azure.com) eller [PowerShell-cmdleten](https://msdn.microsoft.com/library/dn820234.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d950-129">Data developer creates a gateway for an Azure Data Factory using either the [Azure portal](https://portal.azure.com) or [PowerShell Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx).</span></span>
2. <span data-ttu-id="1d950-130">Data utvecklare skapar en länkad tjänst för ett lokalt datalager genom att ange en gateway.</span><span class="sxs-lookup"><span data-stu-id="1d950-130">Data developer creates a linked service for an on-premises data store by specifying the gateway.</span></span> <span data-ttu-id="1d950-131">Som en del av hur du konfigurerar den länkade tjänsten används data utvecklare ställa in autentiseringsuppgifter programmet för att ange typer av autentisering och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1d950-131">As part of setting up the linked service, data developer uses the Setting Credentials application to specify authentication types and credentials.</span></span>  <span data-ttu-id="1d950-132">Dialogrutan ställa in autentiseringsuppgifter program kommunicerar med datalagret att testa anslutningen och gateway för att spara autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1d950-132">The Setting Credentials application dialog communicates with the data store to test connection and the gateway to save credentials.</span></span>
3. <span data-ttu-id="1d950-133">Gateway krypterar autentiseringsuppgifterna med det certifikat som är associerade med denna gateway (anges av data developer) innan du sparar autentiseringsuppgifter i molnet.</span><span class="sxs-lookup"><span data-stu-id="1d950-133">Gateway encrypts the credentials with the certificate associated with the gateway (supplied by data developer), before saving the credentials in the cloud.</span></span>
4. <span data-ttu-id="1d950-134">Data Factory-tjänsten kommunicerar med gateway för schemaläggning och hantering av jobb via en kontrollkanal som använder en delad Azure service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="1d950-134">Data Factory service communicates with the gateway for scheduling & management of jobs via a control channel that uses a shared Azure service bus queue.</span></span> <span data-ttu-id="1d950-135">När en aktivitet kopieringsjobbet måste vara inletts, köar Data Factory begäran tillsammans med information om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1d950-135">When a copy activity job needs to be kicked off, Data Factory queues the request along with credential information.</span></span> <span data-ttu-id="1d950-136">Gateway av systemtillstånd aktiveras jobbet efter avsökning kön.</span><span class="sxs-lookup"><span data-stu-id="1d950-136">Gateway kicks off the job after polling the queue.</span></span>
5. <span data-ttu-id="1d950-137">Gatewayen dekrypterar autentiseringsuppgifter med samma certifikat och sedan ansluter till lokala datalager med rätt autentiseringstyp och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1d950-137">The gateway decrypts the credentials with the same certificate and then connects to the on-premises data store with proper authentication type and credentials.</span></span>
6. <span data-ttu-id="1d950-138">Gatewayen kopierar data från ett lokalt Arkiv till lagringsutrymmet i molnet, och vice versa beroende på hur Kopieringsaktiviteten har konfigurerats i data-pipeline.</span><span class="sxs-lookup"><span data-stu-id="1d950-138">The gateway copies data from an on-premises store to a cloud storage, or vice versa depending on how the Copy Activity is configured in the data pipeline.</span></span> <span data-ttu-id="1d950-139">För det här steget kommunicerar gatewayen direkt med molnbaserade storage-tjänster som Azure Blob Storage via en säker kanal (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1d950-139">For this step, the gateway directly communicates with cloud-based storage services such as Azure Blob Storage over a secure (HTTPS) channel.</span></span>

### <a name="considerations-for-using-gateway"></a><span data-ttu-id="1d950-140">Överväganden för att använda gatewayen</span><span class="sxs-lookup"><span data-stu-id="1d950-140">Considerations for using gateway</span></span>
* <span data-ttu-id="1d950-141">En instans av data management gateway kan användas för flera lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="1d950-141">A single instance of data management gateway can be used for multiple on-premises data sources.</span></span> <span data-ttu-id="1d950-142">Dock **en enda gateway-instans som är knutna till endast en Azure data factory** och kan inte delas med en annan data factory.</span><span class="sxs-lookup"><span data-stu-id="1d950-142">However, **a single gateway instance is tied to only one Azure data factory** and cannot be shared with another data factory.</span></span>
* <span data-ttu-id="1d950-143">Du kan ha **endast en instans av data management gateway** installeras på en enda dator.</span><span class="sxs-lookup"><span data-stu-id="1d950-143">You can have **only one instance of data management gateway** installed on a single machine.</span></span> <span data-ttu-id="1d950-144">Anta att du har två datafabriker som behöver åtkomst till lokala datakällor som du behöver installera gateways på två lokala datorer.</span><span class="sxs-lookup"><span data-stu-id="1d950-144">Suppose, you have two data factories that need to access on-premises data sources, you need to install gateways on two on-premises computers.</span></span> <span data-ttu-id="1d950-145">Med andra ord är en gateway kopplad till en specifik data factory</span><span class="sxs-lookup"><span data-stu-id="1d950-145">In other words, a gateway is tied to a specific data factory</span></span>
* <span data-ttu-id="1d950-146">Den **gateway behöver inte finnas på samma dator som datakällan**.</span><span class="sxs-lookup"><span data-stu-id="1d950-146">The **gateway does not need to be on the same machine as the data source**.</span></span> <span data-ttu-id="1d950-147">Men minskar med gateway närmare till datakällan tiden för gateway för att ansluta till datakällan.</span><span class="sxs-lookup"><span data-stu-id="1d950-147">However, having gateway closer to the data source reduces the time for the gateway to connect to the data source.</span></span> <span data-ttu-id="1d950-148">Vi rekommenderar att du installerar en gateway på en dator som skiljer sig från det som är värd för lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="1d950-148">We recommend that you install the gateway on a machine that is different from the one that hosts on-premises data source.</span></span> <span data-ttu-id="1d950-149">När gatewayen och datakälla finns på olika datorer, konkurrerar gatewayen inte för resurser med datakällan.</span><span class="sxs-lookup"><span data-stu-id="1d950-149">When the gateway and data source are on different machines, the gateway does not compete for resources with data source.</span></span>
* <span data-ttu-id="1d950-150">Du kan ha **flera gateways på olika datorer som ansluter till samma lokala datakälla**.</span><span class="sxs-lookup"><span data-stu-id="1d950-150">You can have **multiple gateways on different machines connecting to the same on-premises data source**.</span></span> <span data-ttu-id="1d950-151">Till exempel du kanske har två gateways som betjänar två datafabriker men samma lokala datakälla har registrerats med båda datafabriker.</span><span class="sxs-lookup"><span data-stu-id="1d950-151">For example, you may have two gateways serving two data factories but the same on-premises data source is registered with both the data factories.</span></span>
* <span data-ttu-id="1d950-152">Om du redan har en gateway som har installerats på din dator fungerar en **Power BI** scenario, installera en **separat gateway för Azure Data Factory** på en annan dator.</span><span class="sxs-lookup"><span data-stu-id="1d950-152">If you already have a gateway installed on your computer serving a **Power BI** scenario, install a **separate gateway for Azure Data Factory** on another machine.</span></span>
* <span data-ttu-id="1d950-153">Gatewayen måste användas även när du använder **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="1d950-153">Gateway must be used even when you use **ExpressRoute**.</span></span>
* <span data-ttu-id="1d950-154">Hantera din datakälla som en lokal datakälla (som finns bakom en brandvägg) även om du använder **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="1d950-154">Treat your data source as an on-premises data source (that is behind a firewall) even when you use **ExpressRoute**.</span></span> <span data-ttu-id="1d950-155">Använda gatewayen för att upprätta en anslutning mellan service och datakällan.</span><span class="sxs-lookup"><span data-stu-id="1d950-155">Use the gateway to establish connectivity between the service and the data source.</span></span>
* <span data-ttu-id="1d950-156">Du måste **använda gateway** även om datalagret finns i molnet på ett **Azure IaaS-VM**.</span><span class="sxs-lookup"><span data-stu-id="1d950-156">You must **use the gateway** even if the data store is in the cloud on an **Azure IaaS VM**.</span></span>

## <a name="installation"></a><span data-ttu-id="1d950-157">Installation</span><span class="sxs-lookup"><span data-stu-id="1d950-157">Installation</span></span>
### <a name="prerequisites"></a><span data-ttu-id="1d950-158">Krav</span><span class="sxs-lookup"><span data-stu-id="1d950-158">Prerequisites</span></span>
* <span data-ttu-id="1d950-159">Den stöds **operativsystemet** versioner är Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="1d950-159">The supported **Operating System** versions are Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span></span> <span data-ttu-id="1d950-160">Installation av data management gateway på en domänkontrollant stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="1d950-160">Installation of the data management gateway on a domain controller is currently not supported.</span></span>
* <span data-ttu-id="1d950-161">.NET framework 4.5.1 eller senare krävs.</span><span class="sxs-lookup"><span data-stu-id="1d950-161">.NET Framework 4.5.1 or above is required.</span></span> <span data-ttu-id="1d950-162">Om du installerar gateway på en Windows 7-dator, installerar du .NET Framework 4.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1d950-162">If you are installing gateway on a Windows 7 machine, install .NET Framework 4.5 or later.</span></span> <span data-ttu-id="1d950-163">Se [systemkrav för .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="1d950-163">See [.NET Framework System Requirements](https://msdn.microsoft.com/library/8z6watww.aspx) for details.</span></span>
* <span data-ttu-id="1d950-164">Den rekommenderade **configuration** för gateway-datorn och är minst 2 GHz, 4 kärnor, 8 GB RAM-minne och 80 GB-disk.</span><span class="sxs-lookup"><span data-stu-id="1d950-164">The recommended **configuration** for the gateway machine is at least 2 GHz, 4 cores, 8-GB RAM, and 80-GB disk.</span></span>
* <span data-ttu-id="1d950-165">Om värddatorn i viloläge, svarar gatewayen inte på databegäranden.</span><span class="sxs-lookup"><span data-stu-id="1d950-165">If the host machine hibernates, the gateway does not respond to data requests.</span></span> <span data-ttu-id="1d950-166">Därför konfigurera en lämplig **energischema** på datorn innan du installerar gateway.</span><span class="sxs-lookup"><span data-stu-id="1d950-166">Therefore, configure an appropriate **power plan** on the computer before installing the gateway.</span></span> <span data-ttu-id="1d950-167">Om datorn är konfigurerad för viloläge, frågar ett meddelande i gateway-installationen.</span><span class="sxs-lookup"><span data-stu-id="1d950-167">If the machine is configured to hibernate, the gateway installation prompts a message.</span></span>
* <span data-ttu-id="1d950-168">Du måste vara administratör på datorn för att installera och konfigurera data management gateway har.</span><span class="sxs-lookup"><span data-stu-id="1d950-168">You must be an administrator on the machine to install and configure the data management gateway successfully.</span></span> <span data-ttu-id="1d950-169">Du kan lägga till fler användare att de **data management gateway användare** lokala Windows-gruppen.</span><span class="sxs-lookup"><span data-stu-id="1d950-169">You can add additional users to the **data management gateway Users** local Windows group.</span></span> <span data-ttu-id="1d950-170">Medlemmar i gruppen ska kunna använda den **Data Management Gateway Configuration Manager** verktyg för att konfigurera gatewayen.</span><span class="sxs-lookup"><span data-stu-id="1d950-170">The members of this group are able to use the **Data Management Gateway Configuration Manager** tool to configure the gateway.</span></span>

<span data-ttu-id="1d950-171">Som uppstår kopiera aktiviteten körs på en specifik frekvens följer Resursanvändning (CPU, minne) på datorn också samma mönster med belastning och ledig tid.</span><span class="sxs-lookup"><span data-stu-id="1d950-171">As copy activity runs happen on a specific frequency, the resource usage (CPU, memory) on the machine also follows the same pattern with peak and idle times.</span></span> <span data-ttu-id="1d950-172">Resursutnyttjande beror också kraftigt på mängden data som flyttas.</span><span class="sxs-lookup"><span data-stu-id="1d950-172">Resource utilization also depends heavily on the amount of data being moved.</span></span> <span data-ttu-id="1d950-173">När flera kopiera jobb pågår, se Resursanvändning gå upp under tider med låg belastning.</span><span class="sxs-lookup"><span data-stu-id="1d950-173">When multiple copy jobs are in progress, you see resource usage go up during peak times.</span></span>

### <a name="installation-options"></a><span data-ttu-id="1d950-174">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="1d950-174">Installation options</span></span>
<span data-ttu-id="1d950-175">Data management gateway kan installeras på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1d950-175">Data management gateway can be installed in the following ways:</span></span>

* <span data-ttu-id="1d950-176">Genom att hämta en MSI-installationspaketet från den [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="1d950-176">By downloading an MSI setup package from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>  <span data-ttu-id="1d950-177">MSI-filerna kan också användas för att uppgradera befintliga data management gateway till den senaste versionen med alla inställningar som bevaras.</span><span class="sxs-lookup"><span data-stu-id="1d950-177">The MSI can also be used to upgrade existing data management gateway to the latest version, with all settings preserved.</span></span>
* <span data-ttu-id="1d950-178">Genom att klicka på **ladda ned och installera datagatewayen** länken under manuell installation eller **installera direkt på den här datorn** under SNABBINSTALLATIONEN.</span><span class="sxs-lookup"><span data-stu-id="1d950-178">By clicking **Download and install data gateway** link under MANUAL SETUP or **Install directly on this computer** under EXPRESS SETUP.</span></span> <span data-ttu-id="1d950-179">Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du använder Expressinstallationen.</span><span class="sxs-lookup"><span data-stu-id="1d950-179">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on using express setup.</span></span> <span data-ttu-id="1d950-180">Den manuella åtgärden går du till download center.</span><span class="sxs-lookup"><span data-stu-id="1d950-180">The manual step takes you to the download center.</span></span>  <span data-ttu-id="1d950-181">Anvisningar för att hämta och installera gatewayen från download center finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d950-181">The instructions for downloading and installing the gateway from download center are in the next section.</span></span>

### <a name="installation-best-practices"></a><span data-ttu-id="1d950-182">Metodtips för installation:</span><span class="sxs-lookup"><span data-stu-id="1d950-182">Installation best practices:</span></span>
1. <span data-ttu-id="1d950-183">Konfigurera energischema på värddatorn för gateway så att datorn inte försättas i viloläge.</span><span class="sxs-lookup"><span data-stu-id="1d950-183">Configure power plan on the host machine for the gateway so that the machine does not hibernate.</span></span> <span data-ttu-id="1d950-184">Om värddatorn i viloläge, svarar gatewayen inte på databegäranden.</span><span class="sxs-lookup"><span data-stu-id="1d950-184">If the host machine hibernates, the gateway does not respond to data requests.</span></span>
2. <span data-ttu-id="1d950-185">Säkerhetskopiera certifikatet som är kopplade till gatewayen.</span><span class="sxs-lookup"><span data-stu-id="1d950-185">Back up the certificate associated with the gateway.</span></span>

### <a name="install-the-gateway-from-download-center"></a><span data-ttu-id="1d950-186">Installera gatewayen från download center</span><span class="sxs-lookup"><span data-stu-id="1d950-186">Install the gateway from download center</span></span>
1. <span data-ttu-id="1d950-187">Gå till [hämtningssidan för Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="1d950-187">Navigate to [Microsoft Data Management Gateway download page](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>
2. <span data-ttu-id="1d950-188">Klicka på **hämta**, Välj lämplig version (**32-bitars** vs. **64-bitars**), och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d950-188">Click **Download**, select the appropriate version (**32-bit** vs. **64-bit**), and click **Next**.</span></span>
3. <span data-ttu-id="1d950-189">Kör den **MSI** direkt eller spara den på hårddisken och kör.</span><span class="sxs-lookup"><span data-stu-id="1d950-189">Run the **MSI** directly or save it to your hard disk and run.</span></span>
4. <span data-ttu-id="1d950-190">På den **Välkommen** väljer en **språk** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d950-190">On the **Welcome** page, select a **language** click **Next**.</span></span>
5. <span data-ttu-id="1d950-191">**Acceptera** licensavtalet och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d950-191">**Accept** the End-User License Agreement and click **Next**.</span></span>
6. <span data-ttu-id="1d950-192">Välj **mappen** installera gatewayen och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d950-192">Select **folder** to install the gateway and click **Next**.</span></span>
7. <span data-ttu-id="1d950-193">På den **redo att installera** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="1d950-193">On the **Ready to install** page, click **Install**.</span></span>
8. <span data-ttu-id="1d950-194">Klicka på **Slutför** att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="1d950-194">Click **Finish** to complete installation.</span></span>
9. <span data-ttu-id="1d950-195">Hämta nyckeln från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1d950-195">Get the key from the Azure portal.</span></span> <span data-ttu-id="1d950-196">Se avsnittet nästa steg för steg-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="1d950-196">See the next section for step-by-step instructions.</span></span>
10. <span data-ttu-id="1d950-197">På den **registrera gateway** sidan **Data Management Gateway Configuration Manager** körs på datorn, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1d950-197">On the **Register gateway** page of **Data Management Gateway Configuration Manager** running on your machine, do the following steps:</span></span>
    1. <span data-ttu-id="1d950-198">Klistra in nyckeln i texten.</span><span class="sxs-lookup"><span data-stu-id="1d950-198">Paste the key in the text.</span></span>
    2. <span data-ttu-id="1d950-199">Du kan också klicka på **visa gatewaynyckeln** att se nyckeltexten.</span><span class="sxs-lookup"><span data-stu-id="1d950-199">Optionally, click **Show gateway key** to see the key text.</span></span>
    3. <span data-ttu-id="1d950-200">Klicka på **registrera**.</span><span class="sxs-lookup"><span data-stu-id="1d950-200">Click **Register**.</span></span>

### <a name="register-gateway-using-key"></a><span data-ttu-id="1d950-201">Registrera gatewayen med nyckeln</span><span class="sxs-lookup"><span data-stu-id="1d950-201">Register gateway using key</span></span>
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a><span data-ttu-id="1d950-202">Om du inte redan skapat en logisk gateway i portalen</span><span class="sxs-lookup"><span data-stu-id="1d950-202">If you haven't already created a logical gateway in the portal</span></span>
<span data-ttu-id="1d950-203">Att skapa en gateway i portalen och hämta nyckeln från den **konfigurera** sidan, Följ stegen från genomgången i den [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1d950-203">To create a gateway in the portal and get the key from the **Configure** page, Follow steps from walkthrough in the [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a><span data-ttu-id="1d950-204">Om du redan har skapat logiska gatewayen i portalen</span><span class="sxs-lookup"><span data-stu-id="1d950-204">If you have already created the logical gateway in the portal</span></span>
1. <span data-ttu-id="1d950-205">I Azure portal, navigerar du till den **Datafabriken** och klicka på **länkade tjänster** panelen.</span><span class="sxs-lookup"><span data-stu-id="1d950-205">In Azure portal, navigate to the **Data Factory** page, and click **Linked Services** tile.</span></span>

    ![Data Factory-sida](media/data-factory-data-management-gateway/data-factory-blade.png)
2. <span data-ttu-id="1d950-207">I den **länkade tjänster** markerar du den logiska **gateway** du skapade i portalen.</span><span class="sxs-lookup"><span data-stu-id="1d950-207">In the **Linked Services** page, select the logical **gateway** you created in the portal.</span></span>

    ![logiska gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. <span data-ttu-id="1d950-209">I den **Datagatewayen** klickar du på **ladda ned och installera datagateway**.</span><span class="sxs-lookup"><span data-stu-id="1d950-209">In the **Data Gateway** page, click **Download and install data gateway**.</span></span>

    ![Ladda ned länken i portalen](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. <span data-ttu-id="1d950-211">I den **konfigurera** klickar du på **återskapa nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="1d950-211">In the **Configure** page, click **Recreate key**.</span></span> <span data-ttu-id="1d950-212">Klicka på Ja i varningsmeddelandet när du har läst den noggrant.</span><span class="sxs-lookup"><span data-stu-id="1d950-212">Click Yes on the warning message after reading it carefully.</span></span>

    ![Återskapa nyckel](media/data-factory-data-management-gateway/recreate-key-button.png)
5. <span data-ttu-id="1d950-214">Klicka på knappen Kopiera bredvid nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1d950-214">Click Copy button next to the key.</span></span> <span data-ttu-id="1d950-215">Nyckeln kopieras till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="1d950-215">The key is copied to the clipboard.</span></span>

    ![Kopiera nyckeln](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a><span data-ttu-id="1d950-217">Ikoner i Systemstatusfältet / meddelanden</span><span class="sxs-lookup"><span data-stu-id="1d950-217">System tray icons/ notifications</span></span>
<span data-ttu-id="1d950-218">Följande bild visar några av Systemstatusfältet som visas.</span><span class="sxs-lookup"><span data-stu-id="1d950-218">The following image shows some of the tray icons that you see.</span></span>

![ikoner i Systemstatusfältet](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

<span data-ttu-id="1d950-220">Om du flyttar markören över system fack ikonen/meddelandet finns information om tillståndet för gateway-/ uppdateringsåtgärden i ett popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="1d950-220">If you move cursor over the system tray icon/notification message, you see details about the state of the gateway/update operation in a popup window.</span></span>

### <a name="ports-and-firewall"></a><span data-ttu-id="1d950-221">Portar och brandvägg</span><span class="sxs-lookup"><span data-stu-id="1d950-221">Ports and firewall</span></span>
<span data-ttu-id="1d950-222">Det finns två brandväggar som du behöver tänka på: **företagsbrandvägg** körs på den centrala routern för organisationen, och **Windows-brandväggen** konfigurerad som en daemon på den lokala datorn där gatewayen är installerad.</span><span class="sxs-lookup"><span data-stu-id="1d950-222">There are two firewalls you need to consider: **corporate firewall** running on the central router of the organization, and **Windows firewall** configured as a daemon on the local machine where the gateway is installed.</span></span>  

![brandväggar](./media/data-factory-data-management-gateway/firewalls2.png)

<span data-ttu-id="1d950-224">På företagets brandvägg nivå måste du konfigurera följande domäner och utgående portar:</span><span class="sxs-lookup"><span data-stu-id="1d950-224">At corporate firewall level, you need configure the following domains and outbound ports:</span></span>

| <span data-ttu-id="1d950-225">Domännamn</span><span class="sxs-lookup"><span data-stu-id="1d950-225">Domain names</span></span> | <span data-ttu-id="1d950-226">Portar</span><span class="sxs-lookup"><span data-stu-id="1d950-226">Ports</span></span> | <span data-ttu-id="1d950-227">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1d950-227">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d950-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="1d950-228">*.servicebus.windows.net</span></span> |<span data-ttu-id="1d950-229">443, 80</span><span class="sxs-lookup"><span data-stu-id="1d950-229">443, 80</span></span> |<span data-ttu-id="1d950-230">Används för kommunikation med Data Movement Service-serverdelen</span><span class="sxs-lookup"><span data-stu-id="1d950-230">Used for communication with Data Movement Service backend</span></span> |
| <span data-ttu-id="1d950-231">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="1d950-231">*.core.windows.net</span></span> |<span data-ttu-id="1d950-232">443</span><span class="sxs-lookup"><span data-stu-id="1d950-232">443</span></span> |<span data-ttu-id="1d950-233">Används för mellanlagrad kopia med Azure Blob (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="1d950-233">Used for Staged copy using Azure Blob (if configured)</span></span>|
| <span data-ttu-id="1d950-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="1d950-234">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="1d950-235">443</span><span class="sxs-lookup"><span data-stu-id="1d950-235">443</span></span> |<span data-ttu-id="1d950-236">Används för kommunikation med Data Movement Service-serverdelen</span><span class="sxs-lookup"><span data-stu-id="1d950-236">Used for communication with Data Movement Service backend</span></span> |


<span data-ttu-id="1d950-237">På Windows-brandväggen nivå aktiveras normalt portarna utgående.</span><span class="sxs-lookup"><span data-stu-id="1d950-237">At Windows firewall level, these outbound ports are normally enabled.</span></span> <span data-ttu-id="1d950-238">Om inte, du kan konfigurera de domäner och portar i enlighet med detta på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="1d950-238">If not, you can configure the domains and ports accordingly on gateway machine.</span></span>

> [!NOTE]
> 1. <span data-ttu-id="1d950-239">Baserat på käll- / sänkor, du kan behöva godkända ytterligare domäner och utgående portar i företagets/Windows-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="1d950-239">Based on your source/ sinks, you may have to whitelist additional domains and outbound ports in your corporate/Windows firewall.</span></span>
> 2. <span data-ttu-id="1d950-240">För vissa databaser i molnet (till exempel: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)osv), du kan behöva godkända IP-adressen för Gateway-datorn på deras brandväggskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1d950-240">For some Cloud Databases (For example: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), etc.), you may need to whitelist IP address of Gateway machine on their firewall configuration.</span></span>
>
>


#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a><span data-ttu-id="1d950-241">Kopiera data från ett dataarkiv som källa till ett sink-datalager</span><span class="sxs-lookup"><span data-stu-id="1d950-241">Copy data from a source data store to a sink data store</span></span>
<span data-ttu-id="1d950-242">Kontrollera att brandväggsregler är aktiverade på korrekt på företagets brandvägg och Windows-brandväggen på gateway-datorn och datalager sig själv.</span><span class="sxs-lookup"><span data-stu-id="1d950-242">Ensure that the firewall rules are enabled properly on the corporate firewall, Windows firewall on the gateway machine, and the data store itself.</span></span> <span data-ttu-id="1d950-243">Om du aktiverar de här reglerna kan gateway att ansluta till både källa och mottagare har.</span><span class="sxs-lookup"><span data-stu-id="1d950-243">Enabling these rules allows the gateway to connect to both source and sink successfully.</span></span> <span data-ttu-id="1d950-244">Aktivera regler för varje datalager som är inblandade i kopieringen.</span><span class="sxs-lookup"><span data-stu-id="1d950-244">Enable rules for each data store that is involved in the copy operation.</span></span>

<span data-ttu-id="1d950-245">Om du vill kopiera från **ett lokalt datalager till en Azure SQL Database-sink eller en Azure SQL Data Warehouse sink**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1d950-245">For example, to copy from **an on-premises data store to an Azure SQL Database sink or an Azure SQL Data Warehouse sink**, do the following steps:</span></span>

* <span data-ttu-id="1d950-246">Tillåt utgående **TCP** -kommunikation på port **1433** för både Windows-brandväggen och företagets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="1d950-246">Allow outbound **TCP** communication on port **1433** for both Windows firewall and corporate firewall.</span></span>
* <span data-ttu-id="1d950-247">Konfigurera brandväggsinställningar för Azure SQL-server för att lägga till IP-adressen för gateway-datorn i listan över tillåtna IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="1d950-247">Configure the firewall settings of Azure SQL server to add the IP address of the gateway machine to the list of allowed IP addresses.</span></span>

> [!NOTE]
> <span data-ttu-id="1d950-248">Om brandväggen inte tillåter att utgående port 1433, Gateway kan inte komma åt Azure SQL direkt.</span><span class="sxs-lookup"><span data-stu-id="1d950-248">If your firewall does not allow outbound port 1433, Gateway can't access Azure SQL directly.</span></span> <span data-ttu-id="1d950-249">I det här fallet kan du använda [mellanlagrad kopiera](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) till SQL Azure Database / SQL Azure DW.</span><span class="sxs-lookup"><span data-stu-id="1d950-249">In this case, you may use [Staged Copy](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) to SQL Azure Database/ SQL Azure DW.</span></span> <span data-ttu-id="1d950-250">I det här scenariot skulle du endast kräva HTTPS (port 443) för flytt av data.</span><span class="sxs-lookup"><span data-stu-id="1d950-250">In this scenario, you would only require HTTPS (port 443) for the data movement.</span></span>
>
>


### <a name="proxy-server-considerations"></a><span data-ttu-id="1d950-251">Proxy server-överväganden</span><span class="sxs-lookup"><span data-stu-id="1d950-251">Proxy server considerations</span></span>
<span data-ttu-id="1d950-252">Om din företagets nätverksmiljö använder en proxyserver för åtkomst till internet, kan du konfigurera data management gateway för att använda rätt proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="1d950-252">If your corporate network environment uses a proxy server to access the internet, configure data management gateway to use appropriate proxy settings.</span></span> <span data-ttu-id="1d950-253">Du kan ange proxyn under fasen registreringen.</span><span class="sxs-lookup"><span data-stu-id="1d950-253">You can set the proxy during the initial registration phase.</span></span>

![Ange proxy under registreringen](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

<span data-ttu-id="1d950-255">Gateway använder proxyservern för att ansluta till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d950-255">Gateway uses the proxy server to connect to the cloud service.</span></span> <span data-ttu-id="1d950-256">Klicka på **ändra** länken under installationen.</span><span class="sxs-lookup"><span data-stu-id="1d950-256">Click **Change** link during initial setup.</span></span> <span data-ttu-id="1d950-257">Du ser den **proxyinställning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1d950-257">You see the **proxy setting** dialog.</span></span>

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxySettings.png)

<span data-ttu-id="1d950-259">Det finns tre konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="1d950-259">There are three configuration options:</span></span>

* <span data-ttu-id="1d950-260">**Använd inte proxyservern**: Gateway inte använder alla proxy uttryckligen för att ansluta till molntjänster.</span><span class="sxs-lookup"><span data-stu-id="1d950-260">**Do not use proxy**: Gateway does not explicitly use any proxy to connect to cloud services.</span></span>
* <span data-ttu-id="1d950-261">**Använd system proxy**: Gateway använder Proxyinställningen som är konfigurerade i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har konfigurerats i diahost.exe.config och diawp.exe.config ansluter gateway till Molntjänsten direkt utan att gå via proxy.</span><span class="sxs-lookup"><span data-stu-id="1d950-261">**Use system proxy**: Gateway uses the proxy setting that is configured in diahost.exe.config and diawp.exe.config.  If no proxy is configured in diahost.exe.config and diawp.exe.config, gateway connects to cloud service directly without going through proxy.</span></span>
* <span data-ttu-id="1d950-262">**Använda anpassade proxy**: konfigurera HTTP-proxyn använder för gateway istället för att använda konfigurationer i diahost.exe.config och diawp.exe.config.  Adress och Port krävs.</span><span class="sxs-lookup"><span data-stu-id="1d950-262">**Use custom proxy**: Configure the HTTP proxy setting to use for gateway, instead of using configurations in diahost.exe.config and diawp.exe.config.  Address and Port are required.</span></span>  <span data-ttu-id="1d950-263">Användarnamn och lösenord är valfria beroende på inställningen för autentisering av din proxyserver.</span><span class="sxs-lookup"><span data-stu-id="1d950-263">User Name and Password are optional depending on your proxy’s authentication setting.</span></span>  <span data-ttu-id="1d950-264">Alla inställningar är krypterad med Autentiseringscertifikatet för gatewayen och lagras lokalt på värddatorn för gateway.</span><span class="sxs-lookup"><span data-stu-id="1d950-264">All settings are encrypted with the credential certificate of the gateway and stored locally on the gateway host machine.</span></span>

<span data-ttu-id="1d950-265">Data management gateway-värdtjänsten startas om automatiskt när du sparar de uppdaterade proxyinställningarna.</span><span class="sxs-lookup"><span data-stu-id="1d950-265">The data management gateway Host Service restarts automatically after you save the updated proxy settings.</span></span>

<span data-ttu-id="1d950-266">När gatewayen har registrerats, om du vill visa eller uppdatera proxyinställningarna, använda Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="1d950-266">After gateway has been successfully registered, if you want to view or update proxy settings, use Data Management Gateway Configuration Manager.</span></span>

1. <span data-ttu-id="1d950-267">Starta **Data Management Gateway Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="1d950-267">Launch **Data Management Gateway Configuration Manager**.</span></span>
2. <span data-ttu-id="1d950-268">Växla till den **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="1d950-268">Switch to the **Settings** tab.</span></span>
3. <span data-ttu-id="1d950-269">Klicka på **ändra** länken i **HTTP-Proxy** avsnittet för att starta den **ange HTTP-Proxy** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1d950-269">Click **Change** link in **HTTP Proxy** section to launch the **Set HTTP Proxy** dialog.</span></span>  
4. <span data-ttu-id="1d950-270">När du klickar på den **nästa** knappen, visas ett varningsmeddelande som ber om din tillåtelse för att spara Proxyinställningen och starta om Gateway-värdtjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d950-270">After you click the **Next** button, you see a warning dialog asking for your permission to save the proxy setting and restart the Gateway Host Service.</span></span>

<span data-ttu-id="1d950-271">Du kan visa och uppdatera HTTP-proxy med verktyget Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="1d950-271">You can view and update HTTP proxy by using Configuration Manager tool.</span></span>

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> <span data-ttu-id="1d950-273">Om du ställer in en proxyserver med NTLM-autentisering, körs Gateway-värdtjänsten under domänkontot.</span><span class="sxs-lookup"><span data-stu-id="1d950-273">If you set up a proxy server with NTLM authentication, Gateway Host Service runs under the domain account.</span></span> <span data-ttu-id="1d950-274">Om du ändrar lösenordet för domänkontot senare, Kom ihåg att uppdatera konfigurationsinställningarna för tjänsten och därefter starta om den.</span><span class="sxs-lookup"><span data-stu-id="1d950-274">If you change the password for the domain account later, remember to update configuration settings for the service and restart it accordingly.</span></span> <span data-ttu-id="1d950-275">På grund av det här kravet rekommenderar vi att du använder ett dedikerat domänkonto att få åtkomst till proxyservern kräver inte att uppdatera lösenordet ofta.</span><span class="sxs-lookup"><span data-stu-id="1d950-275">Due to this requirement, we suggest you use a dedicated domain account to access the proxy server that does not require you to update the password frequently.</span></span>
>
>

### <a name="configure-proxy-server-settings"></a><span data-ttu-id="1d950-276">Konfigurera inställningar för proxyserver</span><span class="sxs-lookup"><span data-stu-id="1d950-276">Configure proxy server settings</span></span>
<span data-ttu-id="1d950-277">Om du väljer **använder system-proxy** inställningen för HTTP-proxyn använder gateway Proxyinställningen i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har angetts i diahost.exe.config och diawp.exe.config ansluter gateway till Molntjänsten direkt utan att gå via proxy.</span><span class="sxs-lookup"><span data-stu-id="1d950-277">If you select **Use system proxy** setting for the HTTP proxy, gateway uses the proxy setting in diahost.exe.config and diawp.exe.config.  If no proxy is specified in diahost.exe.config and diawp.exe.config, gateway connects to cloud service directly without going through proxy.</span></span> <span data-ttu-id="1d950-278">Följande procedur innehåller instruktioner för att uppdatera filen diahost.exe.config.</span><span class="sxs-lookup"><span data-stu-id="1d950-278">The following procedure provides instructions for updating the diahost.exe.config file.</span></span>  

1. <span data-ttu-id="1d950-279">Skapa en säker kopia av C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config att säkerhetskopiera den ursprungliga filen i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="1d950-279">In File Explorer, make a safe copy of C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config to back up the original file.</span></span>
2. <span data-ttu-id="1d950-280">Starta Notepad.exe kör som administratör och öppna filen ”C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. Du hittar Standardetiketten för system.net som visas i följande kod:</span><span class="sxs-lookup"><span data-stu-id="1d950-280">Launch Notepad.exe running as administrator, and open text file “C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. You find the default tag for system.net as shown in the following code:</span></span>

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   <span data-ttu-id="1d950-281">Du kan sedan lägga till proxy serverinformation som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1d950-281">You can then add proxy server details as shown in the following example:</span></span>

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   <span data-ttu-id="1d950-282">Ytterligare egenskaper som är tillåtna i proxy-taggen och ange nödvändiga inställningar som scriptLocation.</span><span class="sxs-lookup"><span data-stu-id="1d950-282">Additional properties are allowed inside the proxy tag to specify the required settings like scriptLocation.</span></span> <span data-ttu-id="1d950-283">Referera till [proxy Element (nätverksinställningar)](https://msdn.microsoft.com/library/sa91de1e.aspx) på syntax.</span><span class="sxs-lookup"><span data-stu-id="1d950-283">Refer to [proxy Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on syntax.</span></span>

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. <span data-ttu-id="1d950-284">Spara konfigurationsfilen till den ursprungliga platsen och sedan starta om tjänsten Data Management Gateway-värdtjänsten hämtar ändringarna.</span><span class="sxs-lookup"><span data-stu-id="1d950-284">Save the configuration file into the original location, then restart the Data Management Gateway Host service, which picks up the changes.</span></span> <span data-ttu-id="1d950-285">Starta om tjänsten: använda tjänster appleten från Kontrollpanelen eller från den **Data Management Gateway Configuration Manager** > klickar du på den **stoppa tjänsten** knappen och klicka sedan på den **Start Tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="1d950-285">To restart the service: use services applet from the control panel, or from the **Data Management Gateway Configuration Manager** > click the **Stop Service** button, then click the **Start Service**.</span></span> <span data-ttu-id="1d950-286">Om tjänsten inte startar, är det troligt att en felaktig syntax för XML-taggen har lagts till i programmets konfigurationsfil som har redigerats.</span><span class="sxs-lookup"><span data-stu-id="1d950-286">If the service does not start, it is likely that an incorrect XML tag syntax has been added into the application configuration file that was edited.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d950-287">Glöm inte att uppdatera **både** diahost.exe.config och diawp.exe.config.</span><span class="sxs-lookup"><span data-stu-id="1d950-287">Do not forget to update **both** diahost.exe.config and diawp.exe.config.</span></span>  


<span data-ttu-id="1d950-288">Förutom dessa punkter måste du också kontrollera att Microsoft Azure finns i ditt företags godkända.</span><span class="sxs-lookup"><span data-stu-id="1d950-288">In addition to these points, you also need to make sure Microsoft Azure is in your company’s whitelist.</span></span> <span data-ttu-id="1d950-289">Listan över giltiga Microsoft Azure-IP-adresser kan hämtas från den [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="1d950-289">The list of valid Microsoft Azure IP addresses can be downloaded from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a><span data-ttu-id="1d950-290">Möjliga problem för brandväggar och proxyservrar serverproblem</span><span class="sxs-lookup"><span data-stu-id="1d950-290">Possible symptoms for firewall and proxy server-related issues</span></span>
<span data-ttu-id="1d950-291">Om du stöter på fel liknande följande dem, är det troligen på grund av felaktig konfiguration av servern brandvägg eller proxyserver, vilket blockerar gateway ansluter till Data Factory för att autentisera sig själv.</span><span class="sxs-lookup"><span data-stu-id="1d950-291">If you encounter errors similar to the following ones, it is likely due to improper configuration of the firewall or proxy server, which blocks gateway from connecting to Data Factory to authenticate itself.</span></span> <span data-ttu-id="1d950-292">Se föregående avsnitt för att se till att brandväggen och proxyservern konfigureras korrekt.</span><span class="sxs-lookup"><span data-stu-id="1d950-292">Refer to previous section to ensure your firewall and proxy server are properly configured.</span></span>

1. <span data-ttu-id="1d950-293">När du försöker registrera gatewayen som du får följande fel: ”Det gick inte att registrera gateway-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1d950-293">When you try to register the gateway, you receive the following error: "Failed to register the gateway key.</span></span> <span data-ttu-id="1d950-294">Innan du försöker registrera gateway-nyckeln igen, bekräfta att data management gateway är i anslutet tillstånd och Data Management Gateway-värdtjänsten har startats ”.</span><span class="sxs-lookup"><span data-stu-id="1d950-294">Before trying to register the gateway key again, confirm that the data management gateway is in a connected state and the Data Management Gateway Host Service is Started."</span></span>
2. <span data-ttu-id="1d950-295">När du öppnar Configuration Manager kan se du status som ”frånkopplad” eller ”ansluter”.</span><span class="sxs-lookup"><span data-stu-id="1d950-295">When you open Configuration Manager, you see status as “Disconnected” or “Connecting.”</span></span> <span data-ttu-id="1d950-296">När du visar Windows-händelseloggar, under ”Loggboken” > ”program och tjänstloggar” > ”Data Management Gateway” felmeddelanden visas till exempel följande fel:`Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span><span class="sxs-lookup"><span data-stu-id="1d950-296">When viewing Windows event logs, under “Event Viewer” > “Application and Services Logs” > “Data Management Gateway”, you see error messages such as the following error: `Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span></span>

### <a name="open-port-8050-for-credential-encryption"></a><span data-ttu-id="1d950-297">Öppna port 8050 för kryptering av autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1d950-297">Open port 8050 for credential encryption</span></span>
<span data-ttu-id="1d950-298">Den **ställa in autentiseringsuppgifter** programmet använder den inkommande porten **8050** vidarebefordrande referenser till gateway när du ställer in en lokal länkad tjänst i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1d950-298">The **Setting Credentials** application uses the inbound port **8050** to relay credentials to the gateway when you set up an on-premises linked service in the Azure portal.</span></span> <span data-ttu-id="1d950-299">Under installationen av gateway öppnas som standard gateway-installationen den på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="1d950-299">During gateway setup, by default, the gateway installation opens it on the gateway machine.</span></span>

<span data-ttu-id="1d950-300">Om du använder en brandvägg från tredje part, kan du öppna port 8050 manuellt.</span><span class="sxs-lookup"><span data-stu-id="1d950-300">If you are using a third-party firewall, you can manually open the port 8050.</span></span> <span data-ttu-id="1d950-301">Om du stöter på problem med brandväggen under installationen av gateway, kan du använda följande kommando för att installera gatewayen utan att konfigurera brandväggen.</span><span class="sxs-lookup"><span data-stu-id="1d950-301">If you run into firewall issue during gateway setup, you can try using the following command to install the gateway without configuring the firewall.</span></span>

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

<span data-ttu-id="1d950-302">Om du inte väljer att öppna port 8050 på gateway-datorn, använda metoder än med hjälp av den **ställa in autentiseringsuppgifter** tillämpningsprogrammet kan konfigurera autentiseringsuppgifterna för datalager.</span><span class="sxs-lookup"><span data-stu-id="1d950-302">If you choose not to open the port 8050 on the gateway machine, use mechanisms other than using the **Setting Credentials** application to configure data store credentials.</span></span> <span data-ttu-id="1d950-303">Du kan till exempel använda [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1d950-303">For example, you could use [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="1d950-304">Se [ställa in autentiseringsuppgifter och säkerhet](#set-credentials-and-securityy) avsnitt om hur data lagra autentiseringsuppgifter som kan anges.</span><span class="sxs-lookup"><span data-stu-id="1d950-304">See [Setting Credentials and Security](#set-credentials-and-securityy) section on how data store credentials can be set.</span></span>

## <a name="update"></a><span data-ttu-id="1d950-305">Uppdatering</span><span class="sxs-lookup"><span data-stu-id="1d950-305">Update</span></span>
<span data-ttu-id="1d950-306">Data management gateway uppdateras automatiskt som standard när en nyare version av gatewayen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="1d950-306">By default, data management gateway is automatically updated when a newer version of the gateway is available.</span></span> <span data-ttu-id="1d950-307">Gatewayen uppdateras inte förrän alla schemalagda aktiviteter är klar.</span><span class="sxs-lookup"><span data-stu-id="1d950-307">The gateway is not updated until all the scheduled tasks are done.</span></span> <span data-ttu-id="1d950-308">Inga ytterligare uppgifter bearbetas av gateway förrän uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="1d950-308">No further tasks are processed by the gateway until the update operation is completed.</span></span> <span data-ttu-id="1d950-309">Om uppdateringen misslyckas återställs gateway till den gamla versionen.</span><span class="sxs-lookup"><span data-stu-id="1d950-309">If the update fails, gateway is rolled back to the old version.</span></span>

<span data-ttu-id="1d950-310">Tidpunkt för schemalagd uppdatering visas på följande platser:</span><span class="sxs-lookup"><span data-stu-id="1d950-310">You see the scheduled update time in the following places:</span></span>

* <span data-ttu-id="1d950-311">Sidan gateway egenskaper i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1d950-311">The gateway properties page in the Azure portal.</span></span>
* <span data-ttu-id="1d950-312">Startsidan för Konfigurationshanteraren för Data Management Gateway</span><span class="sxs-lookup"><span data-stu-id="1d950-312">Home page of the Data Management Gateway Configuration Manager</span></span>
* <span data-ttu-id="1d950-313">System fack-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1d950-313">System tray notification message.</span></span>

<span data-ttu-id="1d950-314">Fliken Start av Konfigurationshanteraren för Data Management Gateway visar schema för uppdatering och senast gatewayen har installerats/uppdatera.</span><span class="sxs-lookup"><span data-stu-id="1d950-314">The Home tab of the Data Management Gateway Configuration Manager displays the update schedule and the last time the gateway was installed/updated.</span></span>

![Schemauppdateringar](media/data-factory-data-management-gateway/UpdateSection.png)

<span data-ttu-id="1d950-316">Du kan installera uppdateringen direkt eller vänta tills gateway uppdateras automatiskt vid den schemalagda tiden.</span><span class="sxs-lookup"><span data-stu-id="1d950-316">You can install the update right away or wait for the gateway to be automatically updated at the scheduled time.</span></span> <span data-ttu-id="1d950-317">Följande bild visar exempelvis meddelandet som visas i Gateway Configuration Manager tillsammans med knappen Uppdatera kan du installera den direkt.</span><span class="sxs-lookup"><span data-stu-id="1d950-317">For example, the following image shows you the notification message shown in the Gateway Configuration Manager along with the Update button that you can click to install it immediately.</span></span>

![Uppdatering i DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

<span data-ttu-id="1d950-319">Meddelande i systemfältet skulle se ut enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="1d950-319">The notification message in the system tray would look as shown in the following image:</span></span>

![Systemmeddelande fack](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

<span data-ttu-id="1d950-321">Du kan se status för uppdateringen (manuell eller automatisk) i systemfältet.</span><span class="sxs-lookup"><span data-stu-id="1d950-321">You see the status of update operation (manual or automatic) in the system tray.</span></span> <span data-ttu-id="1d950-322">När du startar Gateway Configuration Manager nästa gång du ser ett meddelande i meddelandefältet att gatewayen har uppdaterats tillsammans med en länk till [vad är nytt avsnitt](data-factory-gateway-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="1d950-322">When you launch Gateway Configuration Manager next time, you see a message on the notification bar that the gateway has been updated along with a link to [what's new topic](data-factory-gateway-release-notes.md).</span></span>

### <a name="to-disableenable-auto-update-feature"></a><span data-ttu-id="1d950-323">Aktiverar/inaktiverar funktionen för automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="1d950-323">To disable/enable auto-update feature</span></span>
<span data-ttu-id="1d950-324">Du kan inaktivera/aktivera funktionen för automatisk uppdatering genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="1d950-324">You can disable/enable the auto-update feature by doing the following steps:</span></span>

<span data-ttu-id="1d950-325">[För enskild nod gateway]</span><span class="sxs-lookup"><span data-stu-id="1d950-325">[For single node gateway]</span></span>
1. <span data-ttu-id="1d950-326">Starta Windows PowerShell på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="1d950-326">Launch Windows PowerShell on the gateway machine.</span></span>
2. <span data-ttu-id="1d950-327">Växla till mappen C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.</span><span class="sxs-lookup"><span data-stu-id="1d950-327">Switch to the C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="1d950-328">Kör följande kommando för att aktivera automatisk uppdatering funktion av (inaktivera).</span><span class="sxs-lookup"><span data-stu-id="1d950-328">Run the following command to turn the auto-update feature OFF (disable).</span></span>   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. <span data-ttu-id="1d950-329">Om du vill aktivera den igen:</span><span class="sxs-lookup"><span data-stu-id="1d950-329">To turn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
<span data-ttu-id="1d950-330">[[För flera noder hög tillgänglighet och skalbarhet gateway (förhandsgranskning)](data-factory-data-management-gateway-high-availability-scalability.md)]</span><span class="sxs-lookup"><span data-stu-id="1d950-330">[[For multi-node highly available and scalable gateway (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]</span></span>
1. <span data-ttu-id="1d950-331">Starta Windows PowerShell på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="1d950-331">Launch Windows PowerShell on the gateway machine.</span></span>
2. <span data-ttu-id="1d950-332">Växla till mappen C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.</span><span class="sxs-lookup"><span data-stu-id="1d950-332">Switch to the C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="1d950-333">Kör följande kommando för att aktivera automatisk uppdatering funktion av (inaktivera).</span><span class="sxs-lookup"><span data-stu-id="1d950-333">Run the following command to turn the auto-update feature OFF (disable).</span></span>   

    <span data-ttu-id="1d950-334">En extra auktoriseringsnyckel param krävs för gateway med hög tillgänglighet (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="1d950-334">For gateway with high availability feature (preview), an extra AuthKey param is required.</span></span>
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. <span data-ttu-id="1d950-335">Om du vill aktivera den igen:</span><span class="sxs-lookup"><span data-stu-id="1d950-335">To turn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install the gateway, you can launch Data Management Gateway Configuration Manager in one of the following ways:

1. In the **Search** window, type **Data Management Gateway** to access this utility.
2. Run the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
The Home page allows you to do the following actions:

* View status of the gateway (connected to the cloud service etc.).
* **Register** using a key from the portal.
* **Stop** and start the **Data Management Gateway Host service** on the gateway machine.
* **Schedule updates** at a specific time of the days.
* View the date when the gateway was **last updated**.

### Settings page
The Settings page allows you to do the following actions:

* View, change, and export **certificate** used by the gateway. This certificate is used to encrypt data source credentials.
* Change **HTTPS port** for the endpoint. The gateway opens a port for setting the data source credentials.
* **Status** of the endpoint
* View **SSL certificate** is used for SSL communication between portal and the gateway to set credentials for data sources.  

### Diagnostics page
The Diagnostics page allows you to do the following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs to Microsoft if there was a failure.
* **Test connection** to a data source.  

### Help page
The Help page displays the following information:  

* Brief description of the gateway
* Version number
* Links to online help, privacy statement, and license agreement.  

## Monitor gateway in the portal
In the Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate to the home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select the **gateway** in the **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In the **Gateway** page, you can see the memory and CPU usage of the gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** to see more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

The following table provides descriptions of columns in the **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of the logical gateway and nodes associated with the gateway. Node is an on-premises Windows machine that has the gateway installed on it. For information on having more than one node (up to four nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of the logical gateway and the gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows the version of the logical gateway and each gateway node. The version of the logical gateway is determined based on version of majority of nodes in the group. If there are nodes with different versions in the logical gateway setup, only the nodes with the same version number as the logical gateway function properly. Others are in the limited mode and need to be manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies the maximum concurrent jobs for each node. This value is defined based on the machine size. You can increase the limit to scale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when the scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used to execute jobs. There is only one dispatcher node, which is used to pull tasks/jobs from cloud services and dispatch them to different worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in the gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
The following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected to Data Factory service.
Offline | Node is offline.
Upgrading | The node is being auto-updated.
Limited | Due to Connectivity issue. May be due to HTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from the configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect to other nodes. 


The following table provides possible statuses of a **logical gateway**. The gateway status depends on statuses of the gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered to this logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due to credential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure the number of **concurrent data movement jobs** that can run on a node to scale up the capability of moving data between on-premises and cloud data stores. 

When the available memory and CPU are not utilized well, but the idle capacity is 0, you should scale up by increasing the number of concurrent jobs that can run on a node. You may also want to scale up when activities are timing out because the gateway is overloaded. In the advanced settings of a gateway node, you can increase the maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using the data management gateway.  

## Move gateway from one machine to another
This section provides steps for moving gateway client from one machine to another machine.

1. In the portal, navigate to the **Data Factory home page**, and click the **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in the **DATA GATEWAYS** section of the **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In the **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In the **Configure** page, click **Download and install data gateway**, and follow instructions to install the data gateway on the machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep the **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In the **Configure** page in the portal, click **Recreate key** on the command bar, and click **Yes** for the warning message. Click **copy button** next to key text that copies the key to the clipboard. The gateway on the old machine stops functioning as soon you recreate the key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste the **key** into text box in the **Register Gateway** page of the **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box to see the key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** to register the gateway with the cloud service.
9. On the **Settings** tab, click **Change** to select the same certificate that was used with the old gateway, enter the **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from the old gateway by doing the following steps: launch Data Management Gateway Configuration Manager on the old machine, switch to the **Certificate** tab, click **Export** button and follow the instructions.
10. After successful registration of the gateway, you should see the **Registration** set to **Registered** and **Status** set to **Started** on the Home page of the Gateway Configuration Manager.

## Encrypting credentials
To encrypt credentials in the Data Factory Editor, do the following steps:

1. Launch web browser on the **gateway machine**, navigate to [Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in the **DATA FACTORY** page and then click **Author & Deploy** to launch Data Factory Editor.   
2. Click an existing **linked service** in the tree view to see its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In the JSON editor, for the **gatewayName** property, enter the name of the gateway.
4. Enter server name for the **Data Source** property in the **connectionString**.
5. Enter database name for the **Initial Catalog** property in the **connectionString**.    
6. Click **Encrypt** button on the command bar that launches the click-once **Credential Manager** application. You should see the **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In the **Setting Credentials** dialog box, do the following steps:
   1. Select **authentication** that you want the Data Factory service to use to connect to the database.
   2. Enter name of the user who has access to the database for the **USERNAME** setting.
   3. Enter password for the user for the **PASSWORD** setting.  
   4. Click **OK** to encrypt credentials and close the dialog box.
8. You should see a **encryptedCredential** property in the **connectionString** now.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
<span data-ttu-id="1d950-336">Om du har åtkomst till portalen från en dator som skiljer sig från gateway-datorn måste du se till att programmet Autentiseringshanteraren kan ansluta till gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="1d950-336">If you access the portal from a machine that is different from the gateway machine, you must make sure that the Credentials Manager application can connect to the gateway machine.</span></span> <span data-ttu-id="1d950-337">Om programmet inte kan nå gateway-datorn, tillåter den inte att ange autentiseringsuppgifter för datakällan och testa anslutningen till datakällan.</span><span class="sxs-lookup"><span data-stu-id="1d950-337">If the application cannot reach the gateway machine, it does not allow you to set credentials for the data source and to test connection to the data source.</span></span>  

<span data-ttu-id="1d950-338">När du använder den **ställa in autentiseringsuppgifter** program, portalen krypterar autentiseringsuppgifterna med certifikatet som anges i den **certifikat** för den **Gateway Configuration Manager**  på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="1d950-338">When you use the **Setting Credentials** application, the portal encrypts the credentials with the certificate specified in the **Certificate** tab of the **Gateway Configuration Manager** on the gateway machine.</span></span>

<span data-ttu-id="1d950-339">Om du vill söka efter en API-baserad metod för att kryptera autentiseringsuppgifterna, kan du använda den [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet för att kryptera autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="1d950-339">If you are looking for an API-based approach for encrypting the credentials, you can use the [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet to encrypt credentials.</span></span> <span data-ttu-id="1d950-340">Cmdlet använder certifikatet som gatewayen har konfigurerats för att använda för att kryptera autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="1d950-340">The cmdlet uses the certificate that gateway is configured to use to encrypt the credentials.</span></span> <span data-ttu-id="1d950-341">Du lägger till krypterade referenser till den **EncryptedCredential** element i den **connectionString** i JSON.</span><span class="sxs-lookup"><span data-stu-id="1d950-341">You add encrypted credentials to the **EncryptedCredential** element of the **connectionString** in the JSON.</span></span> <span data-ttu-id="1d950-342">Du använder JSON med den [ny AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet eller i den Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="1d950-342">You use the JSON with the [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet or in the Data Factory Editor.</span></span>

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

<span data-ttu-id="1d950-343">Det finns en mer metod för att ange autentiseringsuppgifter med hjälp av Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="1d950-343">There is one more approach for setting credentials using Data Factory Editor.</span></span> <span data-ttu-id="1d950-344">Om du skapar en SQL Server som är länkad tjänst med hjälp av redigeraren och du anger autentiseringsuppgifter i klartext, krypteras autentiseringsuppgifterna med ett certifikat som äger Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d950-344">If you create a SQL Server linked service by using the editor and you enter credentials in plain text, the credentials are encrypted using a certificate that the Data Factory service owns.</span></span> <span data-ttu-id="1d950-345">Den inte använder certifikaten som gatewayen har konfigurerats för att använda.</span><span class="sxs-lookup"><span data-stu-id="1d950-345">It does NOT use the certificate that gateway is configured to use.</span></span> <span data-ttu-id="1d950-346">Den här metoden kan vara lite snabbare i vissa fall, är det mindre säkert.</span><span class="sxs-lookup"><span data-stu-id="1d950-346">While this approach might be a little faster in some cases, it is less secure.</span></span> <span data-ttu-id="1d950-347">Därför rekommenderar vi att du följer den här metoden endast för utveckling/testning.</span><span class="sxs-lookup"><span data-stu-id="1d950-347">Therefore, we recommend that you follow this approach only for development/testing purposes.</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="1d950-348">PowerShell-cmdletar</span><span class="sxs-lookup"><span data-stu-id="1d950-348">PowerShell cmdlets</span></span>
<span data-ttu-id="1d950-349">Det här avsnittet beskrivs hur du skapar och registrera en gateway med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="1d950-349">This section describes how to create and register a gateway using Azure PowerShell cmdlets.</span></span>

1. <span data-ttu-id="1d950-350">Starta **Azure PowerShell** i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="1d950-350">Launch **Azure PowerShell** in administrator mode.</span></span>
2. <span data-ttu-id="1d950-351">Logga in på ditt Azure-konto genom att köra följande kommando och ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="1d950-351">Log in to your Azure account by running the following command and entering your Azure credentials.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="1d950-352">Använd den **ny AzureRmDataFactoryGateway** för att skapa en logisk gateway på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1d950-352">Use the **New-AzureRmDataFactoryGateway** cmdlet to create a logical gateway as follows:</span></span>

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    <span data-ttu-id="1d950-353">**Exempel-kommando och utdata**:</span><span class="sxs-lookup"><span data-stu-id="1d950-353">**Example command and output**:</span></span>

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. <span data-ttu-id="1d950-354">Växla till mappen i Azure PowerShell: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**.</span><span class="sxs-lookup"><span data-stu-id="1d950-354">In Azure PowerShell, switch to the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**.</span></span> <span data-ttu-id="1d950-355">Kör **RegisterGateway.ps1** som är associerade med den lokala variabeln **$Key** som visas i följande kommando.</span><span class="sxs-lookup"><span data-stu-id="1d950-355">Run **RegisterGateway.ps1** associated with the local variable **$Key** as shown in the following command.</span></span> <span data-ttu-id="1d950-356">Det här skriptet registrerar den klientagenten är installerad på datorn med logiska gateway som du har skapat tidigare.</span><span class="sxs-lookup"><span data-stu-id="1d950-356">This script registers the client agent installed on your machine with the logical gateway you create earlier.</span></span>

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    <span data-ttu-id="1d950-357">Du kan registrera gatewayen på en fjärrdator med hjälp av parametern IsRegisterOnRemoteMachine.</span><span class="sxs-lookup"><span data-stu-id="1d950-357">You can register the gateway on a remote machine by using the IsRegisterOnRemoteMachine parameter.</span></span> <span data-ttu-id="1d950-358">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d950-358">Example:</span></span>

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. <span data-ttu-id="1d950-359">Du kan använda den **Get-AzureRmDataFactoryGateway** för att hämta listan över Gateways i din data factory.</span><span class="sxs-lookup"><span data-stu-id="1d950-359">You can use the **Get-AzureRmDataFactoryGateway** cmdlet to get the list of Gateways in your data factory.</span></span> <span data-ttu-id="1d950-360">När den **Status** visar **online**, det innebär att din gateway är klar att användas.</span><span class="sxs-lookup"><span data-stu-id="1d950-360">When the **Status** shows **online**, it means your gateway is ready to use.</span></span>

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
<span data-ttu-id="1d950-361">Du kan ta bort en gateway med hjälp av den **ta bort AzureRmDataFactoryGateway** cmdlet och uppdatera beskrivning för en gateway med hjälp av den **Set AzureRmDataFactoryGateway** cmdlets.</span><span class="sxs-lookup"><span data-stu-id="1d950-361">You can remove a gateway using the **Remove-AzureRmDataFactoryGateway** cmdlet and update description for a gateway using the **Set-AzureRmDataFactoryGateway** cmdlets.</span></span> <span data-ttu-id="1d950-362">Syntax och annan information om dessa cmdlets finns i Cmdlet-referens för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1d950-362">For syntax and other details about these cmdlets, see Data Factory Cmdlet Reference.</span></span>  

### <a name="list-gateways-using-powershell"></a><span data-ttu-id="1d950-363">Visa gateways med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d950-363">List gateways using PowerShell</span></span>

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a><span data-ttu-id="1d950-364">Ta bort gateway med PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d950-364">Remove gateway using PowerShell</span></span>

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a><span data-ttu-id="1d950-365">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d950-365">Next steps</span></span>
* <span data-ttu-id="1d950-366">Se [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1d950-366">See [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="1d950-367">I den här genomgången kan du skapa en pipeline som använder en gateway för att flytta data från en lokal SQL Server-databas till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="1d950-367">In the walkthrough, you create a pipeline that uses the gateway to move data from an on-premises SQL Server database to an Azure blob.</span></span>  
