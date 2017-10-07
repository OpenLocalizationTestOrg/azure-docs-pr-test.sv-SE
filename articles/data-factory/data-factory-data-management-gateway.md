---
title: "aaaData Management Gateway för Data Factory | Microsoft Docs"
description: "Ställ in en data gateway toomove data mellan lokala och hello molnet. Använd Data Management Gateway i Azure Data Factory toomove dina data."
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
ms.openlocfilehash: 6f523891a6230cbc7b407f46f4b02d91dfd2cf46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway"></a><span data-ttu-id="0d2e0-104">Gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="0d2e0-104">Data Management Gateway</span></span>
<span data-ttu-id="0d2e0-105">hello Data management gateway är en klientagent som du måste installera i din lokala miljö toocopy data mellan molnet och lokala datalager.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-105">hello Data management gateway is a client agent that you must install in your on-premises environment toocopy data between cloud and on-premises data stores.</span></span> <span data-ttu-id="0d2e0-106">hello lokala data lagras som stöds av Data Factory listas i hello [datakällor som stöds i](data-factory-data-movement-activities.md#supported-data-stores-and-formats) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-106">hello on-premises data stores supported by Data Factory are listed in hello [Supported data sources](data-factory-data-movement-activities.md#supported-data-stores-and-formats) section.</span></span>

<span data-ttu-id="0d2e0-107">Den här artikeln kompletterar hello genomgången i hello [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-107">This article complements hello walkthrough in hello [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="0d2e0-108">I hello genomgången kan du skapa en pipeline som använder hello gateway toomove data från en lokal SQL Server-databasen tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-108">In hello walkthrough, you create a pipeline that uses hello gateway toomove data from an on-premises SQL Server database tooan Azure blob.</span></span> <span data-ttu-id="0d2e0-109">Den här artikeln innehåller detaljerad information om hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-109">This article provides detailed in-depth information about hello data management gateway.</span></span> 

<span data-ttu-id="0d2e0-110">Du kan skala ut en data management gateway genom att associera flera lokala datorer med hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-110">You can scale out a data management gateway by associating multiple on-premises machines with hello gateway.</span></span> <span data-ttu-id="0d2e0-111">Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-111">You can scale up by increasing number of data movement jobs that can run concurrently on a node.</span></span> <span data-ttu-id="0d2e0-112">Den här funktionen finns också en logisk gateway med en enda nod.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-112">This feature is also available for a logical gateway with a single node.</span></span> <span data-ttu-id="0d2e0-113">Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-113">See [Scaling data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) article for details.</span></span>

> [!NOTE]
> <span data-ttu-id="0d2e0-114">Gateway stöder för närvarande endast hello kopia och lagrade proceduraktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-114">Currently, gateway supports only hello copy activity and stored procedure activity in Data Factory.</span></span> <span data-ttu-id="0d2e0-115">Det är inte möjligt toouse hello gateway från en anpassad aktivitet tooaccess lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-115">It is not possible toouse hello gateway from a custom activity tooaccess on-premises data sources.</span></span>      

## <a name="overview"></a><span data-ttu-id="0d2e0-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="0d2e0-116">Overview</span></span>
### <a name="capabilities-of-data-management-gateway"></a><span data-ttu-id="0d2e0-117">Funktioner i data management gateway</span><span class="sxs-lookup"><span data-stu-id="0d2e0-117">Capabilities of data management gateway</span></span>
<span data-ttu-id="0d2e0-118">Data management gateway ger hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-118">Data management gateway provides hello following capabilities:</span></span>

* <span data-ttu-id="0d2e0-119">Modellen lokala datakällor och datakällor för molnet i hello samma data factory och flytta data.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-119">Model on-premises data sources and cloud data sources within hello same data factory and move data.</span></span>
* <span data-ttu-id="0d2e0-120">Ha en och samma plats för övervakning och hantering med insyn i gatewayens status från hello Data Factory-sida.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-120">Have a single pane of glass for monitoring and management with visibility into gateway status from hello Data Factory page.</span></span>
* <span data-ttu-id="0d2e0-121">Hantera åtkomst tooon lokala datakällor på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-121">Manage access tooon-premises data sources securely.</span></span>
  * <span data-ttu-id="0d2e0-122">Inga ändringar krävs toocorporate brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-122">No changes required toocorporate firewall.</span></span> <span data-ttu-id="0d2e0-123">Gateway blir bara utgående HTTP-baserade anslutningar tooopen internet.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-123">Gateway only makes outbound HTTP-based connections tooopen internet.</span></span>
  * <span data-ttu-id="0d2e0-124">Kryptera autentiseringsuppgifterna för ditt lokala datalager med ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-124">Encrypt credentials for your on-premises data stores with your certificate.</span></span>
* <span data-ttu-id="0d2e0-125">Flytta data effektivt – data överförs i parallella, flexibel toointermittent nätverksproblem med automatisk logik.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-125">Move data efficiently – data is transferred in parallel, resilient toointermittent network issues with auto retry logic.</span></span>

### <a name="command-flow-and-data-flow"></a><span data-ttu-id="0d2e0-126">Kommandot trafikflöde och dataflöde</span><span class="sxs-lookup"><span data-stu-id="0d2e0-126">Command flow and data flow</span></span>
<span data-ttu-id="0d2e0-127">När du använder en kopia toocopy aktivitetsdata mellan lokala och moln använder hello aktiviteten en gateway tootransfer data från lokala datakälla toocloud och vice versa.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-127">When you use a copy activity toocopy data between on-premises and cloud, hello activity uses a gateway tootransfer data from on-premises data source toocloud and vice versa.</span></span>

<span data-ttu-id="0d2e0-128">Här är hello övergripande dataflöde för och översikt över stegen för att kopiera med datagateway: ![dataflöde med gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span><span class="sxs-lookup"><span data-stu-id="0d2e0-128">Here is hello high-level data flow for and summary of steps for copy with data gateway: ![Data flow using gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)</span></span>

1. <span data-ttu-id="0d2e0-129">Data utvecklare skapar en gateway för ett Azure Data Factory med hjälp av antingen hello [Azure-portalen](https://portal.azure.com) eller [PowerShell-cmdleten](https://msdn.microsoft.com/library/dn820234.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-129">Data developer creates a gateway for an Azure Data Factory using either hello [Azure portal](https://portal.azure.com) or [PowerShell Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx).</span></span>
2. <span data-ttu-id="0d2e0-130">Data utvecklare skapar en länkad tjänst för ett lokalt datalager genom att ange hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-130">Data developer creates a linked service for an on-premises data store by specifying hello gateway.</span></span> <span data-ttu-id="0d2e0-131">Som en del av hur du konfigurerar hello länkade tjänsten, använder data developer hello ställa in autentiseringsuppgifter programmet toospecify autentiseringstyper och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-131">As part of setting up hello linked service, data developer uses hello Setting Credentials application toospecify authentication types and credentials.</span></span>  <span data-ttu-id="0d2e0-132">hello ställa in autentiseringsuppgifter dialogrutan programmet kommunicerar med hello data lagra tootest anslutning och hello gateway toosave-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-132">hello Setting Credentials application dialog communicates with hello data store tootest connection and hello gateway toosave credentials.</span></span>
3. <span data-ttu-id="0d2e0-133">Gateway krypterar hello autentiseringsuppgifter med hello certifikat som är associerade med hello gateway (anges av data developer) innan du sparar hello autentiseringsuppgifter i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-133">Gateway encrypts hello credentials with hello certificate associated with hello gateway (supplied by data developer), before saving hello credentials in hello cloud.</span></span>
4. <span data-ttu-id="0d2e0-134">Data Factory-tjänsten kommunicerar med hello gateway för schemaläggning och hantering av jobb via en kontrollkanal som använder en delad Azure service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-134">Data Factory service communicates with hello gateway for scheduling & management of jobs via a control channel that uses a shared Azure service bus queue.</span></span> <span data-ttu-id="0d2e0-135">När en aktivitet kopieringsjobbet måste toobe inletts, köar Data Factory hello begäran tillsammans med information om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-135">When a copy activity job needs toobe kicked off, Data Factory queues hello request along with credential information.</span></span> <span data-ttu-id="0d2e0-136">Gateway av systemtillstånd aktiveras hello jobbet efter avsökning hello kön.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-136">Gateway kicks off hello job after polling hello queue.</span></span>
5. <span data-ttu-id="0d2e0-137">hello gateway dekrypterar hello autentiseringsuppgifter med hello samma certifikat och ansluter sedan toohello lokalt datalager med rätt autentiseringstyp och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-137">hello gateway decrypts hello credentials with hello same certificate and then connects toohello on-premises data store with proper authentication type and credentials.</span></span>
6. <span data-ttu-id="0d2e0-138">hello gateway kopierar data från en lokal store tooa molnlagring eller tvärtom beroende på hur hello Kopieringsaktiviteten är konfigurerad i hello data pipeline.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-138">hello gateway copies data from an on-premises store tooa cloud storage, or vice versa depending on how hello Copy Activity is configured in hello data pipeline.</span></span> <span data-ttu-id="0d2e0-139">För det här steget kommunicerar hello gateway direkt med molnbaserade storage-tjänster som Azure Blob Storage via en säker kanal (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-139">For this step, hello gateway directly communicates with cloud-based storage services such as Azure Blob Storage over a secure (HTTPS) channel.</span></span>

### <a name="considerations-for-using-gateway"></a><span data-ttu-id="0d2e0-140">Överväganden för att använda gatewayen</span><span class="sxs-lookup"><span data-stu-id="0d2e0-140">Considerations for using gateway</span></span>
* <span data-ttu-id="0d2e0-141">En instans av data management gateway kan användas för flera lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-141">A single instance of data management gateway can be used for multiple on-premises data sources.</span></span> <span data-ttu-id="0d2e0-142">Dock **en enda gateway-instans är bundet tooonly ett Azure data factory** och kan inte delas med en annan data factory.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-142">However, **a single gateway instance is tied tooonly one Azure data factory** and cannot be shared with another data factory.</span></span>
* <span data-ttu-id="0d2e0-143">Du kan ha **endast en instans av data management gateway** installeras på en enda dator.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-143">You can have **only one instance of data management gateway** installed on a single machine.</span></span> <span data-ttu-id="0d2e0-144">Anta att du har två datafabriker som behöver tooaccess lokala datakällor måste tooinstall gateways på två lokala datorer.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-144">Suppose, you have two data factories that need tooaccess on-premises data sources, you need tooinstall gateways on two on-premises computers.</span></span> <span data-ttu-id="0d2e0-145">Med andra ord är en gateway bundet tooa specifika data factory</span><span class="sxs-lookup"><span data-stu-id="0d2e0-145">In other words, a gateway is tied tooa specific data factory</span></span>
* <span data-ttu-id="0d2e0-146">Hej **gateway behöver inte toobe på detsamma datorn som datakälla för hello hello**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-146">hello **gateway does not need toobe on hello same machine as hello data source**.</span></span> <span data-ttu-id="0d2e0-147">Med gateway-datakälla för närmare toohello minskar dock hello tid för hello gateway tooconnect toohello datakälla.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-147">However, having gateway closer toohello data source reduces hello time for hello gateway tooconnect toohello data source.</span></span> <span data-ttu-id="0d2e0-148">Vi rekommenderar att du installerar hello gateway på en dator som skiljer sig från hello en värdar lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-148">We recommend that you install hello gateway on a machine that is different from hello one that hosts on-premises data source.</span></span> <span data-ttu-id="0d2e0-149">När hello gateway och en datakälla finns på olika datorer, konkurrerar hello gateway inte för resurser med datakällan.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-149">When hello gateway and data source are on different machines, hello gateway does not compete for resources with data source.</span></span>
* <span data-ttu-id="0d2e0-150">Du kan ha **flera gateways på olika datorer ansluta toohello samma lokala datakälla**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-150">You can have **multiple gateways on different machines connecting toohello same on-premises data source**.</span></span> <span data-ttu-id="0d2e0-151">Du kan till exempel ha två gateways som betjänar två datafabriker men hello samma lokala datakälla har registrerats med både hello datafabriker.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-151">For example, you may have two gateways serving two data factories but hello same on-premises data source is registered with both hello data factories.</span></span>
* <span data-ttu-id="0d2e0-152">Om du redan har en gateway som har installerats på din dator fungerar en **Power BI** scenario, installera en **separat gateway för Azure Data Factory** på en annan dator.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-152">If you already have a gateway installed on your computer serving a **Power BI** scenario, install a **separate gateway for Azure Data Factory** on another machine.</span></span>
* <span data-ttu-id="0d2e0-153">Gatewayen måste användas även när du använder **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-153">Gateway must be used even when you use **ExpressRoute**.</span></span>
* <span data-ttu-id="0d2e0-154">Hantera din datakälla som en lokal datakälla (som finns bakom en brandvägg) även om du använder **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-154">Treat your data source as an on-premises data source (that is behind a firewall) even when you use **ExpressRoute**.</span></span> <span data-ttu-id="0d2e0-155">Använd hello gateway tooestablish anslutningen mellan hello-tjänsten och hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-155">Use hello gateway tooestablish connectivity between hello service and hello data source.</span></span>
* <span data-ttu-id="0d2e0-156">Du måste **använda hello gateway** även om hello datalagret finns i hello moln på en **Azure IaaS-VM**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-156">You must **use hello gateway** even if hello data store is in hello cloud on an **Azure IaaS VM**.</span></span>

## <a name="installation"></a><span data-ttu-id="0d2e0-157">Installation</span><span class="sxs-lookup"><span data-stu-id="0d2e0-157">Installation</span></span>
### <a name="prerequisites"></a><span data-ttu-id="0d2e0-158">Krav</span><span class="sxs-lookup"><span data-stu-id="0d2e0-158">Prerequisites</span></span>
* <span data-ttu-id="0d2e0-159">hello stöds **operativsystemet** versioner är Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-159">hello supported **Operating System** versions are Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2.</span></span> <span data-ttu-id="0d2e0-160">Installation av hello data management gateway på en domänkontrollant stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-160">Installation of hello data management gateway on a domain controller is currently not supported.</span></span>
* <span data-ttu-id="0d2e0-161">.NET framework 4.5.1 eller senare krävs.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-161">.NET Framework 4.5.1 or above is required.</span></span> <span data-ttu-id="0d2e0-162">Om du installerar gateway på en Windows 7-dator, installerar du .NET Framework 4.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-162">If you are installing gateway on a Windows 7 machine, install .NET Framework 4.5 or later.</span></span> <span data-ttu-id="0d2e0-163">Se [systemkrav för .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) mer information.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-163">See [.NET Framework System Requirements](https://msdn.microsoft.com/library/8z6watww.aspx) for details.</span></span>
* <span data-ttu-id="0d2e0-164">hello rekommenderas **configuration** för hello gateway-datorn är minst 2 GHz, 4 kärnor, 8 GB RAM-minne och 80 GB-disk.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-164">hello recommended **configuration** for hello gateway machine is at least 2 GHz, 4 cores, 8-GB RAM, and 80-GB disk.</span></span>
* <span data-ttu-id="0d2e0-165">Om värddatorn hello viloläge svarar hello gateway inte toodata begäranden.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-165">If hello host machine hibernates, hello gateway does not respond toodata requests.</span></span> <span data-ttu-id="0d2e0-166">Därför konfigurera en lämplig **energischema** på hello datorn innan du installerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-166">Therefore, configure an appropriate **power plan** on hello computer before installing hello gateway.</span></span> <span data-ttu-id="0d2e0-167">Om hello datorn är konfigurerade toohibernate, uppmanar hello gateway-installationen ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-167">If hello machine is configured toohibernate, hello gateway installation prompts a message.</span></span>
* <span data-ttu-id="0d2e0-168">Du måste vara administratör på hello datorn tooinstall och konfigurera hello data management gateway har.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-168">You must be an administrator on hello machine tooinstall and configure hello data management gateway successfully.</span></span> <span data-ttu-id="0d2e0-169">Du kan lägga till fler användare toohello **data management gateway användare** lokala Windows-gruppen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-169">You can add additional users toohello **data management gateway Users** local Windows group.</span></span> <span data-ttu-id="0d2e0-170">hello medlemmarna i den här gruppen är kan toouse hello **Data Management Gateway Configuration Manager** verktyget tooconfigure hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-170">hello members of this group are able toouse hello **Data Management Gateway Configuration Manager** tool tooconfigure hello gateway.</span></span>

<span data-ttu-id="0d2e0-171">Som kopierar hända aktiviteten körs på en specifik frekvens, följer hello Resursanvändning (CPU, minne) på hello datorn också hello samma mönster med belastning och ledig tid.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-171">As copy activity runs happen on a specific frequency, hello resource usage (CPU, memory) on hello machine also follows hello same pattern with peak and idle times.</span></span> <span data-ttu-id="0d2e0-172">Resursutnyttjande beror också kraftigt på hello mängden data som flyttas.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-172">Resource utilization also depends heavily on hello amount of data being moved.</span></span> <span data-ttu-id="0d2e0-173">När flera kopiera jobb pågår, se Resursanvändning gå upp under tider med låg belastning.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-173">When multiple copy jobs are in progress, you see resource usage go up during peak times.</span></span>

### <a name="installation-options"></a><span data-ttu-id="0d2e0-174">Installationsalternativ</span><span class="sxs-lookup"><span data-stu-id="0d2e0-174">Installation options</span></span>
<span data-ttu-id="0d2e0-175">Data management gateway kan installeras i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-175">Data management gateway can be installed in hello following ways:</span></span>

* <span data-ttu-id="0d2e0-176">Genom att hämta MSI-installationspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-176">By downloading an MSI setup package from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>  <span data-ttu-id="0d2e0-177">hello MSI kan också vara används tooupgrade befintliga data management gateway toohello senaste versionen, med alla inställningar som bevaras.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-177">hello MSI can also be used tooupgrade existing data management gateway toohello latest version, with all settings preserved.</span></span>
* <span data-ttu-id="0d2e0-178">Genom att klicka på **ladda ned och installera datagatewayen** länken under manuell installation eller **installera direkt på den här datorn** under SNABBINSTALLATIONEN.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-178">By clicking **Download and install data gateway** link under MANUAL SETUP or **Install directly on this computer** under EXPRESS SETUP.</span></span> <span data-ttu-id="0d2e0-179">Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du använder Expressinstallationen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-179">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on using express setup.</span></span> <span data-ttu-id="0d2e0-180">hello manuella steg tar toohello download center.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-180">hello manual step takes you toohello download center.</span></span>  <span data-ttu-id="0d2e0-181">hello anvisningar för att hämta och installera hello gateway från download center finns i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-181">hello instructions for downloading and installing hello gateway from download center are in hello next section.</span></span>

### <a name="installation-best-practices"></a><span data-ttu-id="0d2e0-182">Metodtips för installation:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-182">Installation best practices:</span></span>
1. <span data-ttu-id="0d2e0-183">Konfigurera energischema på hello värddator för hello gateway så att hello datorn inte försättas i viloläge.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-183">Configure power plan on hello host machine for hello gateway so that hello machine does not hibernate.</span></span> <span data-ttu-id="0d2e0-184">Om värddatorn hello viloläge svarar hello gateway inte toodata begäranden.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-184">If hello host machine hibernates, hello gateway does not respond toodata requests.</span></span>
2. <span data-ttu-id="0d2e0-185">Säkerhetskopiera hello certifikat som är associerat med hello-gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-185">Back up hello certificate associated with hello gateway.</span></span>

### <a name="install-hello-gateway-from-download-center"></a><span data-ttu-id="0d2e0-186">Installera hello gateway från download center</span><span class="sxs-lookup"><span data-stu-id="0d2e0-186">Install hello gateway from download center</span></span>
1. <span data-ttu-id="0d2e0-187">Navigera för[hämtningssidan för Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-187">Navigate too[Microsoft Data Management Gateway download page](https://www.microsoft.com/download/details.aspx?id=39717).</span></span>
2. <span data-ttu-id="0d2e0-188">Klicka på **hämta**, Välj hello rätt version (**32-bitars** vs. **64-bitars**), och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-188">Click **Download**, select hello appropriate version (**32-bit** vs. **64-bit**), and click **Next**.</span></span>
3. <span data-ttu-id="0d2e0-189">Kör hello **MSI** direkt eller spara den tooyour hårddisken och kör.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-189">Run hello **MSI** directly or save it tooyour hard disk and run.</span></span>
4. <span data-ttu-id="0d2e0-190">På hello **Välkommen** väljer en **språk** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-190">On hello **Welcome** page, select a **language** click **Next**.</span></span>
5. <span data-ttu-id="0d2e0-191">**Acceptera** hello licensavtalet och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-191">**Accept** hello End-User License Agreement and click **Next**.</span></span>
6. <span data-ttu-id="0d2e0-192">Välj **mappen** tooinstall hello gateway och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-192">Select **folder** tooinstall hello gateway and click **Next**.</span></span>
7. <span data-ttu-id="0d2e0-193">På hello **klar tooinstall** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-193">On hello **Ready tooinstall** page, click **Install**.</span></span>
8. <span data-ttu-id="0d2e0-194">Klicka på **Slutför** toocomplete installation.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-194">Click **Finish** toocomplete installation.</span></span>
9. <span data-ttu-id="0d2e0-195">Hämta hello nyckeln från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-195">Get hello key from hello Azure portal.</span></span> <span data-ttu-id="0d2e0-196">Avsnittet hello nästa steg för steg-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-196">See hello next section for step-by-step instructions.</span></span>
10. <span data-ttu-id="0d2e0-197">På hello **registrera gateway** sidan **Data Management Gateway Configuration Manager** körs på datorn, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-197">On hello **Register gateway** page of **Data Management Gateway Configuration Manager** running on your machine, do hello following steps:</span></span>
    1. <span data-ttu-id="0d2e0-198">Klistra in hello nyckeln i hello text.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-198">Paste hello key in hello text.</span></span>
    2. <span data-ttu-id="0d2e0-199">Du kan också klicka på **visa gatewaynyckeln** toosee hello texten.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-199">Optionally, click **Show gateway key** toosee hello key text.</span></span>
    3. <span data-ttu-id="0d2e0-200">Klicka på **registrera**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-200">Click **Register**.</span></span>

### <a name="register-gateway-using-key"></a><span data-ttu-id="0d2e0-201">Registrera gatewayen med nyckeln</span><span class="sxs-lookup"><span data-stu-id="0d2e0-201">Register gateway using key</span></span>
#### <a name="if-you-havent-already-created-a-logical-gateway-in-hello-portal"></a><span data-ttu-id="0d2e0-202">Om du inte redan skapat en logisk gateway i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="0d2e0-202">If you haven't already created a logical gateway in hello portal</span></span>
<span data-ttu-id="0d2e0-203">toocreate en gateway i hello portal och få hello nyckeln från hello **konfigurera** sidan, Följ stegen från genomgången i hello [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-203">toocreate a gateway in hello portal and get hello key from hello **Configure** page, Follow steps from walkthrough in hello [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>    

#### <a name="if-you-have-already-created-hello-logical-gateway-in-hello-portal"></a><span data-ttu-id="0d2e0-204">Om du redan har skapat logiska hello-gateway i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="0d2e0-204">If you have already created hello logical gateway in hello portal</span></span>
1. <span data-ttu-id="0d2e0-205">Navigera i Azure-portalen toohello **Datafabriken** och klicka på **länkade tjänster** panelen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-205">In Azure portal, navigate toohello **Data Factory** page, and click **Linked Services** tile.</span></span>

    ![Data Factory-sida](media/data-factory-data-management-gateway/data-factory-blade.png)
2. <span data-ttu-id="0d2e0-207">I hello **länkade tjänster** sidan Välj hello logiska **gateway** du skapade i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-207">In hello **Linked Services** page, select hello logical **gateway** you created in hello portal.</span></span>

    ![logiska gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. <span data-ttu-id="0d2e0-209">I hello **Datagatewayen** klickar du på **ladda ned och installera datagateway**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-209">In hello **Data Gateway** page, click **Download and install data gateway**.</span></span>

    ![Hämta länk i hello-portalen](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. <span data-ttu-id="0d2e0-211">I hello **konfigurera** klickar du på **återskapa nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-211">In hello **Configure** page, click **Recreate key**.</span></span> <span data-ttu-id="0d2e0-212">Klicka på Ja hello varningsmeddelande när du har läst den noggrant.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-212">Click Yes on hello warning message after reading it carefully.</span></span>

    ![Återskapa nyckel](media/data-factory-data-management-gateway/recreate-key-button.png)
5. <span data-ttu-id="0d2e0-214">Klicka på Kopiera knappen Nästa toohello nyckel.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-214">Click Copy button next toohello key.</span></span> <span data-ttu-id="0d2e0-215">hello nyckeln är kopierade toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-215">hello key is copied toohello clipboard.</span></span>

    ![Kopiera nyckeln](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a><span data-ttu-id="0d2e0-217">Ikoner i Systemstatusfältet / meddelanden</span><span class="sxs-lookup"><span data-stu-id="0d2e0-217">System tray icons/ notifications</span></span>
<span data-ttu-id="0d2e0-218">hello visar följande bild några av hello ikoner i Systemstatusfältet som visas.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-218">hello following image shows some of hello tray icons that you see.</span></span>

![ikoner i Systemstatusfältet](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

<span data-ttu-id="0d2e0-220">Om du flyttar markören över hello system fack ikonen/meddelandet finns information om hello hello gateway/update-åtgärden i ett popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-220">If you move cursor over hello system tray icon/notification message, you see details about hello state of hello gateway/update operation in a popup window.</span></span>

### <a name="ports-and-firewall"></a><span data-ttu-id="0d2e0-221">Portar och brandvägg</span><span class="sxs-lookup"><span data-stu-id="0d2e0-221">Ports and firewall</span></span>
<span data-ttu-id="0d2e0-222">Det finns två brandväggar som du behöver tooconsider: **företagsbrandvägg** körs på hello centrala routern hello organisation och **Windows-brandväggen** konfigurerad som en daemon på hello lokala dator där hello gatewayen har installerats.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-222">There are two firewalls you need tooconsider: **corporate firewall** running on hello central router of hello organization, and **Windows firewall** configured as a daemon on hello local machine where hello gateway is installed.</span></span>  

![brandväggar](./media/data-factory-data-management-gateway/firewalls2.png)

<span data-ttu-id="0d2e0-224">På företagets brandvägg nivå måste du konfigurera hello följande domäner och utgående portar:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-224">At corporate firewall level, you need configure hello following domains and outbound ports:</span></span>

| <span data-ttu-id="0d2e0-225">Domännamn</span><span class="sxs-lookup"><span data-stu-id="0d2e0-225">Domain names</span></span> | <span data-ttu-id="0d2e0-226">Portar</span><span class="sxs-lookup"><span data-stu-id="0d2e0-226">Ports</span></span> | <span data-ttu-id="0d2e0-227">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0d2e0-227">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0d2e0-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="0d2e0-228">*.servicebus.windows.net</span></span> |<span data-ttu-id="0d2e0-229">443, 80</span><span class="sxs-lookup"><span data-stu-id="0d2e0-229">443, 80</span></span> |<span data-ttu-id="0d2e0-230">Används för kommunikation med Data Movement Service-serverdelen</span><span class="sxs-lookup"><span data-stu-id="0d2e0-230">Used for communication with Data Movement Service backend</span></span> |
| <span data-ttu-id="0d2e0-231">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="0d2e0-231">*.core.windows.net</span></span> |<span data-ttu-id="0d2e0-232">443</span><span class="sxs-lookup"><span data-stu-id="0d2e0-232">443</span></span> |<span data-ttu-id="0d2e0-233">Används för mellanlagrad kopia med Azure Blob (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="0d2e0-233">Used for Staged copy using Azure Blob (if configured)</span></span>|
| <span data-ttu-id="0d2e0-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="0d2e0-234">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="0d2e0-235">443</span><span class="sxs-lookup"><span data-stu-id="0d2e0-235">443</span></span> |<span data-ttu-id="0d2e0-236">Används för kommunikation med Data Movement Service-serverdelen</span><span class="sxs-lookup"><span data-stu-id="0d2e0-236">Used for communication with Data Movement Service backend</span></span> |


<span data-ttu-id="0d2e0-237">På Windows-brandväggen nivå aktiveras normalt portarna utgående.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-237">At Windows firewall level, these outbound ports are normally enabled.</span></span> <span data-ttu-id="0d2e0-238">Om inte, du kan konfigurera hello domäner och portar i enlighet med detta på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-238">If not, you can configure hello domains and ports accordingly on gateway machine.</span></span>

> [!NOTE]
> 1. <span data-ttu-id="0d2e0-239">Baserat på käll- / sänkor, kan du ha toowhitelist ytterligare domäner och utgående portar i företagets/Windows-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-239">Based on your source/ sinks, you may have toowhitelist additional domains and outbound ports in your corporate/Windows firewall.</span></span>
> 2. <span data-ttu-id="0d2e0-240">För vissa databaser i molnet (till exempel: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)osv), du kanske måste toowhitelist IP-adressen för Gateway-datorn på deras brandväggskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-240">For some Cloud Databases (For example: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), etc.), you may need toowhitelist IP address of Gateway machine on their firewall configuration.</span></span>
>
>


#### <a name="copy-data-from-a-source-data-store-tooa-sink-data-store"></a><span data-ttu-id="0d2e0-241">Kopiera data från en källa data store tooa sink datalager</span><span class="sxs-lookup"><span data-stu-id="0d2e0-241">Copy data from a source data store tooa sink data store</span></span>
<span data-ttu-id="0d2e0-242">Kontrollera att hello brandväggsregler är aktiverade på korrekt på hello företagets brandvägg och Windows-brandväggen på hello gateway-datorn och hello datalager sig själv.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-242">Ensure that hello firewall rules are enabled properly on hello corporate firewall, Windows firewall on hello gateway machine, and hello data store itself.</span></span> <span data-ttu-id="0d2e0-243">Om du aktiverar de här reglerna kan hello gateway tooconnect tooboth källa och mottagare har.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-243">Enabling these rules allows hello gateway tooconnect tooboth source and sink successfully.</span></span> <span data-ttu-id="0d2e0-244">Aktivera regler för varje datalager som är involverad i hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-244">Enable rules for each data store that is involved in hello copy operation.</span></span>

<span data-ttu-id="0d2e0-245">Till exempel toocopy från **en lokala data store tooan Azure SQL Database sink eller en Azure SQL Data Warehouse sink**, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-245">For example, toocopy from **an on-premises data store tooan Azure SQL Database sink or an Azure SQL Data Warehouse sink**, do hello following steps:</span></span>

* <span data-ttu-id="0d2e0-246">Tillåt utgående **TCP** -kommunikation på port **1433** för både Windows-brandväggen och företagets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-246">Allow outbound **TCP** communication on port **1433** for both Windows firewall and corporate firewall.</span></span>
* <span data-ttu-id="0d2e0-247">Konfigurera brandväggsinställningar för hello Azure SQL server tooadd hello IP-adressen för hello gateway datorn toohello listan över tillåtna IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-247">Configure hello firewall settings of Azure SQL server tooadd hello IP address of hello gateway machine toohello list of allowed IP addresses.</span></span>

> [!NOTE]
> <span data-ttu-id="0d2e0-248">Om brandväggen inte tillåter att utgående port 1433, Gateway kan inte komma åt Azure SQL direkt.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-248">If your firewall does not allow outbound port 1433, Gateway can't access Azure SQL directly.</span></span> <span data-ttu-id="0d2e0-249">I det här fallet kan du använda [mellanlagrad kopiera](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure-databas / SQL Azure DW.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-249">In this case, you may use [Staged Copy](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure Database/ SQL Azure DW.</span></span> <span data-ttu-id="0d2e0-250">I det här scenariot kan kräver du bara HTTPS (port 443) för hello dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-250">In this scenario, you would only require HTTPS (port 443) for hello data movement.</span></span>
>
>


### <a name="proxy-server-considerations"></a><span data-ttu-id="0d2e0-251">Proxy server-överväganden</span><span class="sxs-lookup"><span data-stu-id="0d2e0-251">Proxy server considerations</span></span>
<span data-ttu-id="0d2e0-252">Om din företagets nätverksmiljö använder en proxy server tooaccess Hej internet, konfigurera inställningar för data management gateway toouse lämplig proxy.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-252">If your corporate network environment uses a proxy server tooaccess hello internet, configure data management gateway toouse appropriate proxy settings.</span></span> <span data-ttu-id="0d2e0-253">Du kan ange hello proxy under hello registreringen fasen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-253">You can set hello proxy during hello initial registration phase.</span></span>

![Ange proxy under registreringen](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

<span data-ttu-id="0d2e0-255">Gateway använder hello proxy server tooconnect toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-255">Gateway uses hello proxy server tooconnect toohello cloud service.</span></span> <span data-ttu-id="0d2e0-256">Klicka på **ändra** länken under installationen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-256">Click **Change** link during initial setup.</span></span> <span data-ttu-id="0d2e0-257">Du ser hello **proxyinställning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-257">You see hello **proxy setting** dialog.</span></span>

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxySettings.png)

<span data-ttu-id="0d2e0-259">Det finns tre konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-259">There are three configuration options:</span></span>

* <span data-ttu-id="0d2e0-260">**Använd inte proxyservern**: Gateway använder inte uttryckligen proxy tooconnect toocloud tjänster.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-260">**Do not use proxy**: Gateway does not explicitly use any proxy tooconnect toocloud services.</span></span>
* <span data-ttu-id="0d2e0-261">**Använd system proxy**: Gateway använder hello proxyinställningar som är konfigurerade i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har konfigurerats i diahost.exe.config och diawp.exe.config ansluter gateway toocloud tjänsten direkt utan att gå via proxy.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-261">**Use system proxy**: Gateway uses hello proxy setting that is configured in diahost.exe.config and diawp.exe.config.  If no proxy is configured in diahost.exe.config and diawp.exe.config, gateway connects toocloud service directly without going through proxy.</span></span>
* <span data-ttu-id="0d2e0-262">**Använda anpassade proxy**: Konfigurera hello HTTP-proxy inställningen toouse för gateway istället för att använda konfigurationer i diahost.exe.config och diawp.exe.config.  Adress och Port krävs.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-262">**Use custom proxy**: Configure hello HTTP proxy setting toouse for gateway, instead of using configurations in diahost.exe.config and diawp.exe.config.  Address and Port are required.</span></span>  <span data-ttu-id="0d2e0-263">Användarnamn och lösenord är valfria beroende på inställningen för autentisering av din proxyserver.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-263">User Name and Password are optional depending on your proxy’s authentication setting.</span></span>  <span data-ttu-id="0d2e0-264">Alla inställningar är krypterad med hello hello gatewayens autentiseringscertifikat och lagras lokalt på värddatorn för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-264">All settings are encrypted with hello credential certificate of hello gateway and stored locally on hello gateway host machine.</span></span>

<span data-ttu-id="0d2e0-265">hello data management gateway-värdtjänsten startas om automatiskt när du har sparat hello uppdateras proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-265">hello data management gateway Host Service restarts automatically after you save hello updated proxy settings.</span></span>

<span data-ttu-id="0d2e0-266">När gatewayen har registrerats, om du vill tooview eller uppdatera proxyinställningarna, använda Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-266">After gateway has been successfully registered, if you want tooview or update proxy settings, use Data Management Gateway Configuration Manager.</span></span>

1. <span data-ttu-id="0d2e0-267">Starta **Data Management Gateway Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-267">Launch **Data Management Gateway Configuration Manager**.</span></span>
2. <span data-ttu-id="0d2e0-268">Växla toohello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-268">Switch toohello **Settings** tab.</span></span>
3. <span data-ttu-id="0d2e0-269">Klicka på **ändra** länken i **HTTP-Proxy** avsnittet toolaunch hello **ange HTTP-Proxy** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-269">Click **Change** link in **HTTP Proxy** section toolaunch hello **Set HTTP Proxy** dialog.</span></span>  
4. <span data-ttu-id="0d2e0-270">När du klickar på hello **nästa** knappen, visas ett varningsmeddelande som frågar för din behörighet toosave hello proxyinställning och starta om hello Gateway-värdtjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-270">After you click hello **Next** button, you see a warning dialog asking for your permission toosave hello proxy setting and restart hello Gateway Host Service.</span></span>

<span data-ttu-id="0d2e0-271">Du kan visa och uppdatera HTTP-proxy med verktyget Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-271">You can view and update HTTP proxy by using Configuration Manager tool.</span></span>

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> <span data-ttu-id="0d2e0-273">Om du ställer in en proxyserver med NTLM-autentisering, körs Gateway-värdtjänsten under hello domänkonto.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-273">If you set up a proxy server with NTLM authentication, Gateway Host Service runs under hello domain account.</span></span> <span data-ttu-id="0d2e0-274">Om du ändrar hello lösenord för hello domänkonto senare komma ihåg tooupdate konfigurationsinställningar för hello-tjänsten och starta om den därefter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-274">If you change hello password for hello domain account later, remember tooupdate configuration settings for hello service and restart it accordingly.</span></span> <span data-ttu-id="0d2e0-275">På grund av toothis krav rekommenderar vi att du använder en särskild domän konto tooaccess hello proxyserver som inte kräver att du tooupdate hello lösenord ofta.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-275">Due toothis requirement, we suggest you use a dedicated domain account tooaccess hello proxy server that does not require you tooupdate hello password frequently.</span></span>
>
>

### <a name="configure-proxy-server-settings"></a><span data-ttu-id="0d2e0-276">Konfigurera inställningar för proxyserver</span><span class="sxs-lookup"><span data-stu-id="0d2e0-276">Configure proxy server settings</span></span>
<span data-ttu-id="0d2e0-277">Om du väljer **använder system-proxy** inställning för hello HTTP-proxy gateway använder hello Proxyinställningen i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har angetts i diahost.exe.config och diawp.exe.config ansluter gateway toocloud tjänsten direkt utan att gå via proxy.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-277">If you select **Use system proxy** setting for hello HTTP proxy, gateway uses hello proxy setting in diahost.exe.config and diawp.exe.config.  If no proxy is specified in diahost.exe.config and diawp.exe.config, gateway connects toocloud service directly without going through proxy.</span></span> <span data-ttu-id="0d2e0-278">hello innehåller följande procedur instruktioner för att uppdatera hello diahost.exe.config fil.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-278">hello following procedure provides instructions for updating hello diahost.exe.config file.</span></span>  

1. <span data-ttu-id="0d2e0-279">Skapa en säker kopia av C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config tooback in hello originalfilen i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-279">In File Explorer, make a safe copy of C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config tooback up hello original file.</span></span>
2. <span data-ttu-id="0d2e0-280">Starta Notepad.exe kör som administratör och öppna filen ”C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. Du hittar hello Standardetiketten för system.net enligt hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-280">Launch Notepad.exe running as administrator, and open text file “C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. You find hello default tag for system.net as shown in hello following code:</span></span>

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   <span data-ttu-id="0d2e0-281">Du kan sedan lägga till proxy serverinformation som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-281">You can then add proxy server details as shown in hello following example:</span></span>

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   <span data-ttu-id="0d2e0-282">Ytterligare egenskaper som tillåts i hello taggen toospecify hello krävs proxyinställningar som scriptLocation.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-282">Additional properties are allowed inside hello proxy tag toospecify hello required settings like scriptLocation.</span></span> <span data-ttu-id="0d2e0-283">Se för[proxy Element (nätverksinställningar)](https://msdn.microsoft.com/library/sa91de1e.aspx) på syntax.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-283">Refer too[proxy Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on syntax.</span></span>

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. <span data-ttu-id="0d2e0-284">Spara konfigurationsfilen hello i hello ursprungsplatsen, starta sedan hello Data Management Gateway-värdtjänsten tjänsten, som hämtar hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-284">Save hello configuration file into hello original location, then restart hello Data Management Gateway Host service, which picks up hello changes.</span></span> <span data-ttu-id="0d2e0-285">toorestart hello service: använda tjänster appleten från hello Kontrollpanelen eller från hello **Data Management Gateway Configuration Manager** > klickar du på hello **stoppa tjänsten** knappen och klicka sedan på hello **Starta tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-285">toorestart hello service: use services applet from hello control panel, or from hello **Data Management Gateway Configuration Manager** > click hello **Stop Service** button, then click hello **Start Service**.</span></span> <span data-ttu-id="0d2e0-286">Om hello-tjänsten inte startar, är det troligt att en felaktig syntax för XML-taggen har lagts till hello programmets konfigurationsfil som har redigerats.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-286">If hello service does not start, it is likely that an incorrect XML tag syntax has been added into hello application configuration file that was edited.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0d2e0-287">Glöm inte tooupdate **både** diahost.exe.config och diawp.exe.config.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-287">Do not forget tooupdate **both** diahost.exe.config and diawp.exe.config.</span></span>  


<span data-ttu-id="0d2e0-288">Dessutom toothese punkter, behöver du också toomake till Microsoft Azure finns i ditt företags godkända.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-288">In addition toothese points, you also need toomake sure Microsoft Azure is in your company’s whitelist.</span></span> <span data-ttu-id="0d2e0-289">hello lista över giltiga Microsoft Azure-IP-adresser kan hämtas från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-289">hello list of valid Microsoft Azure IP addresses can be downloaded from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a><span data-ttu-id="0d2e0-290">Möjliga problem för brandväggar och proxyservrar serverproblem</span><span class="sxs-lookup"><span data-stu-id="0d2e0-290">Possible symptoms for firewall and proxy server-related issues</span></span>
<span data-ttu-id="0d2e0-291">Om det uppstår fel liknande toohello följande viktiga, är det troligt på grund av tooimproper hello brandvägg eller proxyserver serverns konfiguration, vilket hindrar gatewayen från anslutande tooData Factory tooauthenticate sig själv.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-291">If you encounter errors similar toohello following ones, it is likely due tooimproper configuration of hello firewall or proxy server, which blocks gateway from connecting tooData Factory tooauthenticate itself.</span></span> <span data-ttu-id="0d2e0-292">Läs tooprevious avsnittet tooensure brandväggar och proxyservrar servern är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-292">Refer tooprevious section tooensure your firewall and proxy server are properly configured.</span></span>

1. <span data-ttu-id="0d2e0-293">När du försöker tooregister hello gateway får du hello följande fel: ”Det gick inte tooregister hello gateway-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-293">When you try tooregister hello gateway, you receive hello following error: "Failed tooregister hello gateway key.</span></span> <span data-ttu-id="0d2e0-294">Innan du försöker tooregister hello gateway-nyckeln igen, bekräfta att hello data management gateway är i anslutet tillstånd och hello Data Management Gateway-värdtjänsten har startats ”.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-294">Before trying tooregister hello gateway key again, confirm that hello data management gateway is in a connected state and hello Data Management Gateway Host Service is Started."</span></span>
2. <span data-ttu-id="0d2e0-295">När du öppnar Configuration Manager kan se du status som ”frånkopplad” eller ”ansluter”.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-295">When you open Configuration Manager, you see status as “Disconnected” or “Connecting.”</span></span> <span data-ttu-id="0d2e0-296">När du visar Windows-händelseloggar, under ”Loggboken” > ”program och tjänstloggar” > ”Data Management Gateway” felmeddelanden visas till exempel hello följande fel:`Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span><span class="sxs-lookup"><span data-stu-id="0d2e0-296">When viewing Windows event logs, under “Event Viewer” > “Application and Services Logs” > “Data Management Gateway”, you see error messages such as hello following error: `Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`</span></span>

### <a name="open-port-8050-for-credential-encryption"></a><span data-ttu-id="0d2e0-297">Öppna port 8050 för kryptering av autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="0d2e0-297">Open port 8050 for credential encryption</span></span>
<span data-ttu-id="0d2e0-298">Hej **ställa in autentiseringsuppgifter** program använder hello inkommande port **8050** toorelay autentiseringsuppgifter toohello gateway när du ställer in en lokal länkade tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-298">hello **Setting Credentials** application uses hello inbound port **8050** toorelay credentials toohello gateway when you set up an on-premises linked service in hello Azure portal.</span></span> <span data-ttu-id="0d2e0-299">Under installationen av gateway öppnas som standard hello gateway-installationen den på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-299">During gateway setup, by default, hello gateway installation opens it on hello gateway machine.</span></span>

<span data-ttu-id="0d2e0-300">Om du använder en brandvägg från tredje part, kan du öppna hello port 8050 manuellt.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-300">If you are using a third-party firewall, you can manually open hello port 8050.</span></span> <span data-ttu-id="0d2e0-301">Om du stöter på problem med brandväggen under installationen av gateway, kan du försöka hello efter kommandot tooinstall hello gateway utan att konfigurera hello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-301">If you run into firewall issue during gateway setup, you can try using hello following command tooinstall hello gateway without configuring hello firewall.</span></span>

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

<span data-ttu-id="0d2e0-302">Om du väljer inte tooopen hello port 8050 på hello gateway-datorn kan använda metoder än med hjälp av hello **ställa in autentiseringsuppgifter** tooconfigure programdata lagra autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-302">If you choose not tooopen hello port 8050 on hello gateway machine, use mechanisms other than using hello **Setting Credentials** application tooconfigure data store credentials.</span></span> <span data-ttu-id="0d2e0-303">Du kan till exempel använda [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-303">For example, you could use [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="0d2e0-304">Se [ställa in autentiseringsuppgifter och säkerhet](#set-credentials-and-securityy) avsnitt om hur data lagra autentiseringsuppgifter som kan anges.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-304">See [Setting Credentials and Security](#set-credentials-and-securityy) section on how data store credentials can be set.</span></span>

## <a name="update"></a><span data-ttu-id="0d2e0-305">Uppdatering</span><span class="sxs-lookup"><span data-stu-id="0d2e0-305">Update</span></span>
<span data-ttu-id="0d2e0-306">Data management gateway uppdateras automatiskt som standard när en nyare version av hello gatewayen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-306">By default, data management gateway is automatically updated when a newer version of hello gateway is available.</span></span> <span data-ttu-id="0d2e0-307">hello gateway uppdateras inte förrän alla hello schemalagda aktiviteter är klar.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-307">hello gateway is not updated until all hello scheduled tasks are done.</span></span> <span data-ttu-id="0d2e0-308">Inga ytterligare uppgifter bearbetas av hello gateway förrän hello uppdateringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-308">No further tasks are processed by hello gateway until hello update operation is completed.</span></span> <span data-ttu-id="0d2e0-309">Om hello uppdatering misslyckas återställs gateway toohello gamla versionen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-309">If hello update fails, gateway is rolled back toohello old version.</span></span>

<span data-ttu-id="0d2e0-310">Du ser hello schemalagda Uppdateringstid i hello följande platser:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-310">You see hello scheduled update time in hello following places:</span></span>

* <span data-ttu-id="0d2e0-311">hello gateway egenskapssidan i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-311">hello gateway properties page in hello Azure portal.</span></span>
* <span data-ttu-id="0d2e0-312">Startsidan för hello Data Management Gateway Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="0d2e0-312">Home page of hello Data Management Gateway Configuration Manager</span></span>
* <span data-ttu-id="0d2e0-313">System fack-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-313">System tray notification message.</span></span>

<span data-ttu-id="0d2e0-314">hello fliken för start av hello Data Management Gateway Configuration Manager visar hello uppdateringsschema och hello senaste gången hello gateway har installerats/uppdatera.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-314">hello Home tab of hello Data Management Gateway Configuration Manager displays hello update schedule and hello last time hello gateway was installed/updated.</span></span>

![Schemauppdateringar](media/data-factory-data-management-gateway/UpdateSection.png)

<span data-ttu-id="0d2e0-316">Du kan installera hello uppdateringen direkt eller vänta tills hello gateway toobe uppdateras automatiskt vid hello schemalagda tillfälle.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-316">You can install hello update right away or wait for hello gateway toobe automatically updated at hello scheduled time.</span></span> <span data-ttu-id="0d2e0-317">Till exempel hello följande bild visar hello av meddelande som visas i hello Gateway Configuration Manager tillsammans med hello uppdateringsknappen som du kan klicka på tooinstall den omedelbart.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-317">For example, hello following image shows you hello notification message shown in hello Gateway Configuration Manager along with hello Update button that you can click tooinstall it immediately.</span></span>

![Uppdatering i DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

<span data-ttu-id="0d2e0-319">hello-meddelande i systemfältet hello skulle se ut enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-319">hello notification message in hello system tray would look as shown in hello following image:</span></span>

![Systemmeddelande fack](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

<span data-ttu-id="0d2e0-321">Du ser hello status för uppdateringen (manuell eller automatisk) i hello systemfältet.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-321">You see hello status of update operation (manual or automatic) in hello system tray.</span></span> <span data-ttu-id="0d2e0-322">När du startar Gateway Configuration Manager nästa gång du ser ett meddelande på hello meddelandefält som hello-gateway har uppdaterats tillsammans med en länk för[vad är nytt avsnitt](data-factory-gateway-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-322">When you launch Gateway Configuration Manager next time, you see a message on hello notification bar that hello gateway has been updated along with a link too[what's new topic](data-factory-gateway-release-notes.md).</span></span>

### <a name="toodisableenable-auto-update-feature"></a><span data-ttu-id="0d2e0-323">toodisable/aktivera funktionen för automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="0d2e0-323">toodisable/enable auto-update feature</span></span>
<span data-ttu-id="0d2e0-324">Du kan inaktivera/aktivera funktionen för automatisk uppdatering av hello genom att göra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-324">You can disable/enable hello auto-update feature by doing hello following steps:</span></span>

<span data-ttu-id="0d2e0-325">[För enskild nod gateway]</span><span class="sxs-lookup"><span data-stu-id="0d2e0-325">[For single node gateway]</span></span>
1. <span data-ttu-id="0d2e0-326">Starta Windows PowerShell på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-326">Launch Windows PowerShell on hello gateway machine.</span></span>
2. <span data-ttu-id="0d2e0-327">Växla toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript mapp.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-327">Switch toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="0d2e0-328">Kör hello efter kommandot tooturn hello automatisk uppdatering funktion av (inaktivera).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-328">Run hello following command tooturn hello auto-update feature OFF (disable).</span></span>   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. <span data-ttu-id="0d2e0-329">tooturn tillbaka på:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-329">tooturn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
<span data-ttu-id="0d2e0-330">[[För flera noder hög tillgänglighet och skalbarhet gateway (förhandsgranskning)](data-factory-data-management-gateway-high-availability-scalability.md)]</span><span class="sxs-lookup"><span data-stu-id="0d2e0-330">[[For multi-node highly available and scalable gateway (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]</span></span>
1. <span data-ttu-id="0d2e0-331">Starta Windows PowerShell på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-331">Launch Windows PowerShell on hello gateway machine.</span></span>
2. <span data-ttu-id="0d2e0-332">Växla toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript mapp.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-332">Switch toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript folder.</span></span>
3. <span data-ttu-id="0d2e0-333">Kör hello efter kommandot tooturn hello automatisk uppdatering funktion av (inaktivera).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-333">Run hello following command tooturn hello auto-update feature OFF (disable).</span></span>   

    <span data-ttu-id="0d2e0-334">En extra auktoriseringsnyckel param krävs för gateway med hög tillgänglighet (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="0d2e0-334">For gateway with high availability feature (preview), an extra AuthKey param is required.</span></span>
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. <span data-ttu-id="0d2e0-335">tooturn tillbaka på:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-335">tooturn it back on:</span></span>

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install hello gateway, you can launch Data Management Gateway Configuration Manager in one of hello following ways:

1. In hello **Search** window, type **Data Management Gateway** tooaccess this utility.
2. Run hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
hello Home page allows you toodo hello following actions:

* View status of hello gateway (connected toohello cloud service etc.).
* **Register** using a key from hello portal.
* **Stop** and start hello **Data Management Gateway Host service** on hello gateway machine.
* **Schedule updates** at a specific time of hello days.
* View hello date when hello gateway was **last updated**.

### Settings page
hello Settings page allows you toodo hello following actions:

* View, change, and export **certificate** used by hello gateway. This certificate is used tooencrypt data source credentials.
* Change **HTTPS port** for hello endpoint. hello gateway opens a port for setting hello data source credentials.
* **Status** of hello endpoint
* View **SSL certificate** is used for SSL communication between portal and hello gateway tooset credentials for data sources.  

### Diagnostics page
hello Diagnostics page allows you toodo hello following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs tooMicrosoft if there was a failure.
* **Test connection** tooa data source.  

### Help page
hello Help page displays hello following information:  

* Brief description of hello gateway
* Version number
* Links tooonline help, privacy statement, and license agreement.  

## Monitor gateway in hello portal
In hello Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate toohello home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select hello **gateway** in hello **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In hello **Gateway** page, you can see hello memory and CPU usage of hello gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** toosee more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

hello following table provides descriptions of columns in hello **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of hello logical gateway and nodes associated with hello gateway. Node is an on-premises Windows machine that has hello gateway installed on it. For information on having more than one node (up toofour nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of hello logical gateway and hello gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows hello version of hello logical gateway and each gateway node. hello version of hello logical gateway is determined based on version of majority of nodes in hello group. If there are nodes with different versions in hello logical gateway setup, only hello nodes with hello same version number as hello logical gateway function properly. Others are in hello limited mode and need toobe manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies hello maximum concurrent jobs for each node. This value is defined based on hello machine size. You can increase hello limit tooscale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when hello scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used tooexecute jobs. There is only one dispatcher node, which is used toopull tasks/jobs from cloud services and dispatch them toodifferent worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in hello gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
hello following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected tooData Factory service.
Offline | Node is offline.
Upgrading | hello node is being auto-updated.
Limited | Due tooConnectivity issue. May be due tooHTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from hello configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect tooother nodes. 


hello following table provides possible statuses of a **logical gateway**. hello gateway status depends on statuses of hello gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered toothis logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due toocredential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure hello number of **concurrent data movement jobs** that can run on a node tooscale up hello capability of moving data between on-premises and cloud data stores. 

When hello available memory and CPU are not utilized well, but hello idle capacity is 0, you should scale up by increasing hello number of concurrent jobs that can run on a node. You may also want tooscale up when activities are timing out because hello gateway is overloaded. In hello advanced settings of a gateway node, you can increase hello maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using hello data management gateway.  

## Move gateway from one machine tooanother
This section provides steps for moving gateway client from one machine tooanother machine.

1. In hello portal, navigate toohello **Data Factory home page**, and click hello **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in hello **DATA GATEWAYS** section of hello **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In hello **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In hello **Configure** page, click **Download and install data gateway**, and follow instructions tooinstall hello data gateway on hello machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep hello **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In hello **Configure** page in hello portal, click **Recreate key** on hello command bar, and click **Yes** for hello warning message. Click **copy button** next tookey text that copies hello key toohello clipboard. hello gateway on hello old machine stops functioning as soon you recreate hello key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste hello **key** into text box in hello **Register Gateway** page of hello **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box toosee hello key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** tooregister hello gateway with hello cloud service.
9. On hello **Settings** tab, click **Change** tooselect hello same certificate that was used with hello old gateway, enter hello **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from hello old gateway by doing hello following steps: launch Data Management Gateway Configuration Manager on hello old machine, switch toohello **Certificate** tab, click **Export** button and follow hello instructions.
10. After successful registration of hello gateway, you should see hello **Registration** set too**Registered** and **Status** set too**Started** on hello Home page of hello Gateway Configuration Manager.

## Encrypting credentials
tooencrypt credentials in hello Data Factory Editor, do hello following steps:

1. Launch web browser on hello **gateway machine**, navigate too[Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in hello **DATA FACTORY** page and then click **Author & Deploy** toolaunch Data Factory Editor.   
2. Click an existing **linked service** in hello tree view toosee its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In hello JSON editor, for hello **gatewayName** property, enter hello name of hello gateway.
4. Enter server name for hello **Data Source** property in hello **connectionString**.
5. Enter database name for hello **Initial Catalog** property in hello **connectionString**.    
6. Click **Encrypt** button on hello command bar that launches hello click-once **Credential Manager** application. You should see hello **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In hello **Setting Credentials** dialog box, do hello following steps:
   1. Select **authentication** that you want hello Data Factory service toouse tooconnect toohello database.
   2. Enter name of hello user who has access toohello database for hello **USERNAME** setting.
   3. Enter password for hello user for hello **PASSWORD** setting.  
   4. Click **OK** tooencrypt credentials and close hello dialog box.
8. You should see a **encryptedCredential** property in hello **connectionString** now.

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
<span data-ttu-id="0d2e0-336">Om du öppnar hello portalen från en dator som skiljer sig från hello gateway-datorn måste du se till att hello Autentiseringshanteraren program kan ansluta toohello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-336">If you access hello portal from a machine that is different from hello gateway machine, you must make sure that hello Credentials Manager application can connect toohello gateway machine.</span></span> <span data-ttu-id="0d2e0-337">Om programmet hello inte kan nå hello gateway-datorn, tillåter den inte tooset autentiseringsuppgifter för datakällan hello och tootest anslutning toohello datakälla.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-337">If hello application cannot reach hello gateway machine, it does not allow you tooset credentials for hello data source and tootest connection toohello data source.</span></span>  

<span data-ttu-id="0d2e0-338">När du använder hello **ställa in autentiseringsuppgifter** programmet hello portal krypterar hello autentiseringsuppgifter med hello-certifikatet som anges i hello **certifikat** för hello **Gateway Configuration Manager** på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-338">When you use hello **Setting Credentials** application, hello portal encrypts hello credentials with hello certificate specified in hello **Certificate** tab of hello **Gateway Configuration Manager** on hello gateway machine.</span></span>

<span data-ttu-id="0d2e0-339">Om du letar efter en API-baserad metod för att kryptera hello autentiseringsuppgifter, kan du använda hello [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet tooencrypt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-339">If you are looking for an API-based approach for encrypting hello credentials, you can use hello [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet tooencrypt credentials.</span></span> <span data-ttu-id="0d2e0-340">hello cmdlet använder hello certifikat som gatewayen är konfigurerad toouse tooencrypt hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-340">hello cmdlet uses hello certificate that gateway is configured toouse tooencrypt hello credentials.</span></span> <span data-ttu-id="0d2e0-341">Du lägger till krypterade autentiseringsuppgifter toohello **EncryptedCredential** element av hello **connectionString** i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-341">You add encrypted credentials toohello **EncryptedCredential** element of hello **connectionString** in hello JSON.</span></span> <span data-ttu-id="0d2e0-342">Du använder hello JSON med hello [ny AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet eller i hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-342">You use hello JSON with hello [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet or in hello Data Factory Editor.</span></span>

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

<span data-ttu-id="0d2e0-343">Det finns en mer metod för att ange autentiseringsuppgifter med hjälp av Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-343">There is one more approach for setting credentials using Data Factory Editor.</span></span> <span data-ttu-id="0d2e0-344">Om du skapar en SQL-Server som är länkad tjänst med hjälp av hello-redigeraren och du ange autentiseringsuppgifter i klartext, hello autentiseringsuppgifterna krypteras med ett certifikat som äger hello Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-344">If you create a SQL Server linked service by using hello editor and you enter credentials in plain text, hello credentials are encrypted using a certificate that hello Data Factory service owns.</span></span> <span data-ttu-id="0d2e0-345">Hello-certifikat som gatewayen är konfigurerad toouse används inte.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-345">It does NOT use hello certificate that gateway is configured toouse.</span></span> <span data-ttu-id="0d2e0-346">Den här metoden kan vara lite snabbare i vissa fall, är det mindre säkert.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-346">While this approach might be a little faster in some cases, it is less secure.</span></span> <span data-ttu-id="0d2e0-347">Därför rekommenderar vi att du följer den här metoden endast för utveckling/testning.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-347">Therefore, we recommend that you follow this approach only for development/testing purposes.</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="0d2e0-348">PowerShell-cmdletar</span><span class="sxs-lookup"><span data-stu-id="0d2e0-348">PowerShell cmdlets</span></span>
<span data-ttu-id="0d2e0-349">Det här avsnittet beskrivs hur toocreate och registrera en gateway med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-349">This section describes how toocreate and register a gateway using Azure PowerShell cmdlets.</span></span>

1. <span data-ttu-id="0d2e0-350">Starta **Azure PowerShell** i administratörsläge.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-350">Launch **Azure PowerShell** in administrator mode.</span></span>
2. <span data-ttu-id="0d2e0-351">Logga in tooyour Azure-konto genom att köra hello följande kommando och ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-351">Log in tooyour Azure account by running hello following command and entering your Azure credentials.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="0d2e0-352">Använd hello **ny AzureRmDataFactoryGateway** cmdlet toocreate logiska gateway på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-352">Use hello **New-AzureRmDataFactoryGateway** cmdlet toocreate a logical gateway as follows:</span></span>

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    <span data-ttu-id="0d2e0-353">**Exempel-kommando och utdata**:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-353">**Example command and output**:</span></span>

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

1. <span data-ttu-id="0d2e0-354">Växla toohello mapp i Azure PowerShell: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-354">In Azure PowerShell, switch toohello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**.</span></span> <span data-ttu-id="0d2e0-355">Kör **RegisterGateway.ps1** som är associerade med hello lokal variabel **$Key** enligt hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-355">Run **RegisterGateway.ps1** associated with hello local variable **$Key** as shown in hello following command.</span></span> <span data-ttu-id="0d2e0-356">Det här skriptet registrerar hello klient-agenten är installerad på datorn med hello logiska gateway som du har skapat tidigare.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-356">This script registers hello client agent installed on your machine with hello logical gateway you create earlier.</span></span>

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    <span data-ttu-id="0d2e0-357">Du kan registrera hello gateway på en fjärrdator med hjälp av hello IsRegisterOnRemoteMachine parameter.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-357">You can register hello gateway on a remote machine by using hello IsRegisterOnRemoteMachine parameter.</span></span> <span data-ttu-id="0d2e0-358">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0d2e0-358">Example:</span></span>

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. <span data-ttu-id="0d2e0-359">Du kan använda hello **Get-AzureRmDataFactoryGateway** cmdlet tooget hello lista över Gateways i din data factory.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-359">You can use hello **Get-AzureRmDataFactoryGateway** cmdlet tooget hello list of Gateways in your data factory.</span></span> <span data-ttu-id="0d2e0-360">När hello **Status** visar **online**, det innebär att din gateway är klar toouse.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-360">When hello **Status** shows **online**, it means your gateway is ready toouse.</span></span>

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
<span data-ttu-id="0d2e0-361">Du kan ta bort en gateway med hello **ta bort AzureRmDataFactoryGateway** cmdlet och uppdatera beskrivning för en gateway med hello **Set AzureRmDataFactoryGateway** cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-361">You can remove a gateway using hello **Remove-AzureRmDataFactoryGateway** cmdlet and update description for a gateway using hello **Set-AzureRmDataFactoryGateway** cmdlets.</span></span> <span data-ttu-id="0d2e0-362">Syntax och annan information om dessa cmdlets finns i Cmdlet-referens för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-362">For syntax and other details about these cmdlets, see Data Factory Cmdlet Reference.</span></span>  

### <a name="list-gateways-using-powershell"></a><span data-ttu-id="0d2e0-363">Visa gateways med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d2e0-363">List gateways using PowerShell</span></span>

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a><span data-ttu-id="0d2e0-364">Ta bort gateway med PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d2e0-364">Remove gateway using PowerShell</span></span>

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a><span data-ttu-id="0d2e0-365">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d2e0-365">Next steps</span></span>
* <span data-ttu-id="0d2e0-366">Se [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-366">See [Move data between on-premises and cloud data stores](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="0d2e0-367">I hello genomgången kan du skapa en pipeline som använder hello gateway toomove data från en lokal SQL Server-databasen tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="0d2e0-367">In hello walkthrough, you create a pipeline that uses hello gateway toomove data from an on-premises SQL Server database tooan Azure blob.</span></span>  
