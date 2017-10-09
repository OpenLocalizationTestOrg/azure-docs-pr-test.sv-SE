---
title: aaaRun Apache Sqoop jobb med Azure HDInsight (Hadoop) | Microsoft Docs
description: "Lär dig hur toouse Azure PowerShell från en arbetsstation toorun Sqoop importera och exportera mellan ett Hadoop-kluster och en Azure SQL database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bdac507704937d77921c9c13d70aa2434c7e3be4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a><span data-ttu-id="92b18-103">Använda Sqoop med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="92b18-103">Use Sqoop with Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="92b18-104">Lär dig hur toouse Sqoop i HDInsight tooimport och exportera mellan HDInsight-kluster och Azure SQL database eller SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-104">Learn how toouse Sqoop in HDInsight tooimport and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

<span data-ttu-id="92b18-105">Även om Hadoop är en fysisk val för bearbetning av Ostrukturerade och halvstrukturerade data, till exempel loggar och filer, kan det också vara en behovet tooprocess strukturerade data som lagras i relationsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="92b18-105">Although Hadoop is a natural choice for processing unstructured and semistructured data, such as logs and files, there may also be a need tooprocess structured data that is stored in relational databases.</span></span>

<span data-ttu-id="92b18-106">[Sqoop] [ sqoop-user-guide-1.4.4] är ett verktyg tootransfer data mellan Hadoop-kluster och relationsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="92b18-106">[Sqoop][sqoop-user-guide-1.4.4] is a tool designed tootransfer data between Hadoop clusters and relational databases.</span></span> <span data-ttu-id="92b18-107">Du kan använda den tooimport data från ett relationella databashanteringssystem (RDBMS) som SQL Server, MySQL eller Oracle till hello Hadoop distributed file system (HDFS), transformera hello data i Hadoop med MapReduce eller Hive och sedan exportera hello data tillbaka till en RDBMS.</span><span class="sxs-lookup"><span data-stu-id="92b18-107">You can use it tooimport data from a relational database management system (RDBMS) such as SQL Server, MySQL, or Oracle into hello Hadoop distributed file system (HDFS), transform hello data in Hadoop with MapReduce or Hive, and then export hello data back into an RDBMS.</span></span> <span data-ttu-id="92b18-108">I kursen får använder du en SQL Server-databas för dina relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="92b18-108">In this tutorial, you are using a SQL Server database for your relational database.</span></span>

<span data-ttu-id="92b18-109">Sqoop-versioner som stöds på HDInsight-kluster finns [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight?][hdinsight-versions]</span><span class="sxs-lookup"><span data-stu-id="92b18-109">For Sqoop versions that are supported on HDInsight clusters, see [What's new in hello cluster versions provided by HDInsight?][hdinsight-versions]</span></span>

## <a name="understand-hello-scenario"></a><span data-ttu-id="92b18-110">Förstå hello scenario</span><span class="sxs-lookup"><span data-stu-id="92b18-110">Understand hello scenario</span></span>

<span data-ttu-id="92b18-111">HDInsight-kluster levereras med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="92b18-111">HDInsight cluster comes with some sample data.</span></span> <span data-ttu-id="92b18-112">Du kan använda följande två samplingarna hello:</span><span class="sxs-lookup"><span data-stu-id="92b18-112">You use hello following two samples:</span></span>

* <span data-ttu-id="92b18-113">En loggfil för log4j som finns på */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="92b18-113">A log4j log file, which is located at */example/data/sample.log*.</span></span> <span data-ttu-id="92b18-114">hello följande loggar extraheras från hello-filen:</span><span class="sxs-lookup"><span data-stu-id="92b18-114">hello following logs are extracted from hello file:</span></span>
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* <span data-ttu-id="92b18-115">En Hive-tabell med namnet *hivesampletable*, vilket referenser hello datafilen på */hive/warehouse/hivesampletable*.</span><span class="sxs-lookup"><span data-stu-id="92b18-115">A Hive table named *hivesampletable*, which references hello data file located at */hive/warehouse/hivesampletable*.</span></span> <span data-ttu-id="92b18-116">hello tabellen innehåller vissa mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="92b18-116">hello table contains some mobile device data.</span></span> 
  
  | <span data-ttu-id="92b18-117">Fält</span><span class="sxs-lookup"><span data-stu-id="92b18-117">Field</span></span> | <span data-ttu-id="92b18-118">Datatyp</span><span class="sxs-lookup"><span data-stu-id="92b18-118">Data type</span></span> |
  | --- | --- |
  | <span data-ttu-id="92b18-119">clientid</span><span class="sxs-lookup"><span data-stu-id="92b18-119">clientid</span></span> |<span data-ttu-id="92b18-120">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-120">string</span></span> |
  | <span data-ttu-id="92b18-121">querytime</span><span class="sxs-lookup"><span data-stu-id="92b18-121">querytime</span></span> |<span data-ttu-id="92b18-122">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-122">string</span></span> |
  | <span data-ttu-id="92b18-123">marknaden</span><span class="sxs-lookup"><span data-stu-id="92b18-123">market</span></span> |<span data-ttu-id="92b18-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-124">string</span></span> |
  | <span data-ttu-id="92b18-125">deviceplatform</span><span class="sxs-lookup"><span data-stu-id="92b18-125">deviceplatform</span></span> |<span data-ttu-id="92b18-126">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-126">string</span></span> |
  | <span data-ttu-id="92b18-127">devicemake</span><span class="sxs-lookup"><span data-stu-id="92b18-127">devicemake</span></span> |<span data-ttu-id="92b18-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-128">string</span></span> |
  | <span data-ttu-id="92b18-129">devicemodel</span><span class="sxs-lookup"><span data-stu-id="92b18-129">devicemodel</span></span> |<span data-ttu-id="92b18-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-130">string</span></span> |
  | <span data-ttu-id="92b18-131">state</span><span class="sxs-lookup"><span data-stu-id="92b18-131">state</span></span> |<span data-ttu-id="92b18-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-132">string</span></span> |
  | <span data-ttu-id="92b18-133">Land</span><span class="sxs-lookup"><span data-stu-id="92b18-133">country</span></span> |<span data-ttu-id="92b18-134">Sträng</span><span class="sxs-lookup"><span data-stu-id="92b18-134">string</span></span> |
  | <span data-ttu-id="92b18-135">querydwelltime</span><span class="sxs-lookup"><span data-stu-id="92b18-135">querydwelltime</span></span> |<span data-ttu-id="92b18-136">dubbla</span><span class="sxs-lookup"><span data-stu-id="92b18-136">double</span></span> |
  | <span data-ttu-id="92b18-137">sessions-ID</span><span class="sxs-lookup"><span data-stu-id="92b18-137">sessionid</span></span> |<span data-ttu-id="92b18-138">bigint</span><span class="sxs-lookup"><span data-stu-id="92b18-138">bigint</span></span> |
  | <span data-ttu-id="92b18-139">sessionpagevieworder</span><span class="sxs-lookup"><span data-stu-id="92b18-139">sessionpagevieworder</span></span> |<span data-ttu-id="92b18-140">bigint</span><span class="sxs-lookup"><span data-stu-id="92b18-140">bigint</span></span> |

<span data-ttu-id="92b18-141">Först måste du exportera *sample.log* och *hivesampletable* toohello Azure SQL database eller tooSQL Server och sedan importera hello tabell som innehåller hello mobil enhetsdata tillbaka tooHDInsight med hjälp av hello följande sökväg:</span><span class="sxs-lookup"><span data-stu-id="92b18-141">First, you export *sample.log* and *hivesampletable* toohello Azure SQL database or tooSQL Server, and then import hello table that contains hello mobile device data back tooHDInsight by using hello following path:</span></span>

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a><span data-ttu-id="92b18-142">Skapa kluster och SQL-databas</span><span class="sxs-lookup"><span data-stu-id="92b18-142">Create cluster and SQL database</span></span>
<span data-ttu-id="92b18-143">Det här avsnittet visar hur toocreate ett kluster, en SQL-databas och hello SQL-databas scheman för körs hello självstudiekurs om att använda hello Azure-portalen och en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="92b18-143">This section shows you how toocreate a cluster, a SQL Database, and hello SQL database schemas for running hello tutorial using hello Azure portal and an Azure Resource Manager template.</span></span> <span data-ttu-id="92b18-144">hello-mallen finns i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="92b18-144">hello template can be found in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/).</span></span> <span data-ttu-id="92b18-145">hello Resource Manager-mall anropar en bacpac paketet toodeploy hello tabell scheman tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-145">hello Resource Manager template calls a bacpac package toodeploy hello table schemas tooSQL database.</span></span>  <span data-ttu-id="92b18-146">Hej bacpac paketet finns i den offentliga blob-behållaren https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span><span class="sxs-lookup"><span data-stu-id="92b18-146">hello bacpac package is located in a public blob container, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac.</span></span> <span data-ttu-id="92b18-147">Om du vill toouse privata behållare för hello bacpac filer, använder du följande värden i hello mallen hello:</span><span class="sxs-lookup"><span data-stu-id="92b18-147">If you want toouse a private container for hello bacpac files, use hello following values in hello template:</span></span>
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

<span data-ttu-id="92b18-148">Om du föredrar toouse Azure PowerShell toocreate hello klustret och hello SQL-databas finns [bilaga A](#appendix-a---a-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="92b18-148">If you prefer toouse Azure PowerShell toocreate hello cluster and hello SQL Database, see [Appendix A](#appendix-a---a-powershell-sample).</span></span>

1. <span data-ttu-id="92b18-149">Klicka på hello följande bild tooopen en Resource Manager-mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="92b18-149">Click hello following image tooopen a Resource Manager template in hello Azure portal.</span></span>         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   

2. <span data-ttu-id="92b18-150">Ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="92b18-150">Enter hello following properties:</span></span>

    - <span data-ttu-id="92b18-151">**Prenumerationen**: Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="92b18-151">**Subscription**: Enter your Azure subscription.</span></span>
    - <span data-ttu-id="92b18-152">**Resursgruppen**: skapa en ny Azure-resursgrupp eller välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="92b18-152">**Resource Group**: Create a new Azure Resource Group, or select an existing Resource Group.</span></span>  <span data-ttu-id="92b18-153">En resursgrupp är för hantering av ändamål.</span><span class="sxs-lookup"><span data-stu-id="92b18-153">A Resource Group is for management purpose.</span></span>  <span data-ttu-id="92b18-154">Det är en behållare för objekt.</span><span class="sxs-lookup"><span data-stu-id="92b18-154">It is a container for objects.</span></span>
    - <span data-ttu-id="92b18-155">**Plats**: Välj en region.</span><span class="sxs-lookup"><span data-stu-id="92b18-155">**Location**: Select a region.</span></span>
    - <span data-ttu-id="92b18-156">**Klusternamn**: Ange ett namn för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="92b18-156">**ClusterName**: Enter a name for hello Hadoop cluster.</span></span>
    - <span data-ttu-id="92b18-157">**Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är administratör.</span><span class="sxs-lookup"><span data-stu-id="92b18-157">**Cluster login name and password**: hello default login name is admin.</span></span>
    - <span data-ttu-id="92b18-158">**SSH-användarnamn och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="92b18-158">**SSH user name and password**.</span></span>
    - <span data-ttu-id="92b18-159">**Databasen i SQL server-inloggningsnamnet och lösenordet**.</span><span class="sxs-lookup"><span data-stu-id="92b18-159">**SQL database server login name and password**.</span></span>
    - <span data-ttu-id="92b18-160">**_artifacts plats**: Använd hello standardvärdet om du vill toouse egna backpac-filen på en annan plats.</span><span class="sxs-lookup"><span data-stu-id="92b18-160">**_artifacts Location**: Use hello default value unless you want toouse your own backpac file in a different location.</span></span>
    - <span data-ttu-id="92b18-161">**_artifacts plats Sas-Token**: lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="92b18-161">**_artifacts Location Sas Token**: Leave it blank.</span></span>
    - <span data-ttu-id="92b18-162">**Bacpac filnamn**: Använd hello standardvärdet om du inte vill toouse backpac filen.</span><span class="sxs-lookup"><span data-stu-id="92b18-162">**Bacpac File Name**: Use hello default value unless you want toouse your own backpac file.</span></span>
     
     <span data-ttu-id="92b18-163">hello följande värden är hårdkodad hello variabler under:</span><span class="sxs-lookup"><span data-stu-id="92b18-163">hello following values are hardcoded in hello variables section:</span></span>
     
     | <span data-ttu-id="92b18-164">Standard lagringskontonamn</span><span class="sxs-lookup"><span data-stu-id="92b18-164">Default storage account name</span></span> | <span data-ttu-id="92b18-165"><CluterName>lagra</span><span class="sxs-lookup"><span data-stu-id="92b18-165"><CluterName>store</span></span> |
     | --- | --- |
     | <span data-ttu-id="92b18-166">Azure SQL server-databasnamn</span><span class="sxs-lookup"><span data-stu-id="92b18-166">Azure SQL database server name</span></span> |<span data-ttu-id="92b18-167"><ClusterName>dbserver</span><span class="sxs-lookup"><span data-stu-id="92b18-167"><ClusterName>dbserver</span></span> |
     | <span data-ttu-id="92b18-168">Azure SQL-databasnamn</span><span class="sxs-lookup"><span data-stu-id="92b18-168">Azure SQL database name</span></span> |<span data-ttu-id="92b18-169"><ClusterName>DB</span><span class="sxs-lookup"><span data-stu-id="92b18-169"><ClusterName>db</span></span> |
     
     <span data-ttu-id="92b18-170">Skriv ned dessa värden.</span><span class="sxs-lookup"><span data-stu-id="92b18-170">Please write down these values.</span></span>  <span data-ttu-id="92b18-171">Du behöver dem senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="92b18-171">You need them later in hello tutorial.</span></span>

<span data-ttu-id="92b18-172">3 Klicka på **OK** toosave hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="92b18-172">3.Click **OK** toosave hello parameters.</span></span>

<span data-ttu-id="92b18-173">4 från hello **anpassad distribution** bladet, klickar du på **resursgruppen** nedrullningsbara rutan och klicka på **ny** toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="92b18-173">4.From hello **Custom deployment** blade, click **Resource group** dropdown box, and then click **New** toocreate a new resource group.</span></span> <span data-ttu-id="92b18-174">hello resursgrupp är en behållare som grupperar hello klustret, hello beroende lagringskontot och andra länkade resurser.</span><span class="sxs-lookup"><span data-stu-id="92b18-174">hello resource group is a container that groups hello cluster, hello dependent storage account and other linked resource.</span></span>

<span data-ttu-id="92b18-175">5. Klicka på **Juridiska villkor** och sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="92b18-175">5.Click **Legal terms**, and then click **Create**.</span></span>

<span data-ttu-id="92b18-176">6. Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="92b18-176">6.Click **Create**.</span></span> <span data-ttu-id="92b18-177">Du ser en ny panel med rubriken skicka distribution för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="92b18-177">You see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="92b18-178">Det tar cirka 20 minuter toocreate hello klustret och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-178">It takes about around 20 minutes toocreate hello cluster and SQL database.</span></span>

<span data-ttu-id="92b18-179">Om du väljer toouse befintliga Azure SQL-databas eller Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="92b18-179">If you choose toouse existing Azure SQL database or Microsoft SQL Server</span></span>

* <span data-ttu-id="92b18-180">**Azure SQL database**: du måste konfigurera en brandväggsregel för hello Azure SQL database server tooallow åtkomst från din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="92b18-180">**Azure SQL database**: You must configure a firewall rule for hello Azure SQL database server tooallow access from your workstation.</span></span> <span data-ttu-id="92b18-181">Instruktioner om hur du skapar en Azure SQL database och konfigurerar hello brandväggen finns [komma igång med Azure SQL database][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="92b18-181">For instructions about creating an Azure SQL database and configuring hello firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="92b18-182">Som standard tillåter en Azure SQL database anslutningar från Azure-tjänster, till exempel Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="92b18-182">By default an Azure SQL database allows connections from Azure services, such as Azure HDInsight.</span></span> <span data-ttu-id="92b18-183">Om brandväggsinställningen är inaktiverad måste tooenable från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="92b18-183">If this firewall setting is disabled, you need tooenable it from hello Azure portal.</span></span> <span data-ttu-id="92b18-184">Instruktioner om hur du skapar en Azure SQL database och konfigurera brandväggsregler, se [skapa och konfigurera SQL-databas][sqldatabase-create-configue].</span><span class="sxs-lookup"><span data-stu-id="92b18-184">For instruction about creating an Azure SQL database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-create-configue].</span></span>
  > 
  > 
* <span data-ttu-id="92b18-185">**SQL Server**: om HDInsight-kluster på hello samma virtuella nätverk i Azure som SQL Server, kan du använda hello stegen i den här artikeln tooimport och exportera data tooa SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-185">**SQL Server**: If your HDInsight cluster is on hello same virtual network in Azure as SQL Server, you can use hello steps in this article tooimport and export data tooa SQL Server database.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="92b18-186">HDInsight stöder plats-baserade virtuella nätverk endast och den för närvarande fungerar inte med tillhörighetsgrupp-baserade virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="92b18-186">HDInsight supports only location-based virtual networks, and it does not currently work with affinity group-based virtual networks.</span></span>
  > 
  > 
  
  * <span data-ttu-id="92b18-187">toocreate och konfigurera ett virtuellt nätverk, se [skapa ett virtuellt nätverk med hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="92b18-187">toocreate and configure a virtual network, see [Create a virtual network using hello Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>
    
    * <span data-ttu-id="92b18-188">När du använder SQL Server i ditt datacenter, måste du konfigurera hello virtuella nätverk som *plats-till-plats* eller *punkt-till-plats*.</span><span class="sxs-lookup"><span data-stu-id="92b18-188">When you are using SQL Server in your datacenter, you must configure hello virtual network as *site-to-site* or *point-to-site*.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="92b18-189">För **punkt-till-plats** virtuella nätverk, SQL Server måste köra hello VPN client configuration program, som är tillgängliga från hello **instrumentpanelen** av konfigurationen av virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="92b18-189">For **point-to-site** virtual networks, SQL Server must be running hello VPN client configuration application, which is available from hello **Dashboard** of your Azure virtual network configuration.</span></span>
      > 
      > 
    * <span data-ttu-id="92b18-190">När du använder SQL Server på en virtuell Azure-dator, konfiguration av virtuellt nätverk kan användas om hello virtuell dator som värd för SQL Server är en medlem av hello samma virtuella nätverk som HDInsight.</span><span class="sxs-lookup"><span data-stu-id="92b18-190">When you are using SQL Server on an Azure virtual machine, any virtual network configuration can be used if hello virtual machine hosting SQL Server is a member of hello same virtual network as HDInsight.</span></span>
  * <span data-ttu-id="92b18-191">toocreate ett HDInsight-kluster på ett virtuellt nätverk finns [skapa Hadoop-kluster i HDInsight med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="92b18-191">toocreate an HDInsight cluster on a virtual network, see [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="92b18-192">SQL Server måste även tillåta autentisering.</span><span class="sxs-lookup"><span data-stu-id="92b18-192">SQL Server must also allow authentication.</span></span> <span data-ttu-id="92b18-193">Du måste använda en SQL Server inloggningen toocomplete hello stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="92b18-193">You must use a SQL Server login toocomplete hello steps in this article.</span></span>
    > 
    > 

## <a name="run-sqoop-jobs"></a><span data-ttu-id="92b18-194">Kör Sqoop jobb</span><span class="sxs-lookup"><span data-stu-id="92b18-194">Run Sqoop jobs</span></span>
<span data-ttu-id="92b18-195">HDInsight kan köra Sqoop jobb med hjälp av olika metoder.</span><span class="sxs-lookup"><span data-stu-id="92b18-195">HDInsight can run Sqoop jobs by using a variety of methods.</span></span> <span data-ttu-id="92b18-196">Använd hello efter tabellen toodecide vilken metod som passar dig sedan länken hello en genomgång.</span><span class="sxs-lookup"><span data-stu-id="92b18-196">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="92b18-197">**Använd den här** om du vill...</span><span class="sxs-lookup"><span data-stu-id="92b18-197">**Use this** if you want...</span></span> | <span data-ttu-id="92b18-198">.. .an **interaktiva** shell</span><span class="sxs-lookup"><span data-stu-id="92b18-198">...an **interactive** shell</span></span> | <span data-ttu-id="92b18-199">... **batch** bearbetning</span><span class="sxs-lookup"><span data-stu-id="92b18-199">...**batch** processing</span></span> | <span data-ttu-id="92b18-200">... med detta **klustret operativsystem**</span><span class="sxs-lookup"><span data-stu-id="92b18-200">...with this **cluster operating system**</span></span> | <span data-ttu-id="92b18-201">.. .from detta **klientoperativsystem**</span><span class="sxs-lookup"><span data-stu-id="92b18-201">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="92b18-202">SSH</span><span class="sxs-lookup"><span data-stu-id="92b18-202">SSH</span></span>](hdinsight-use-sqoop-mac-linux.md) |<span data-ttu-id="92b18-203">✔</span><span class="sxs-lookup"><span data-stu-id="92b18-203">✔</span></span> |<span data-ttu-id="92b18-204">✔</span><span class="sxs-lookup"><span data-stu-id="92b18-204">✔</span></span> |<span data-ttu-id="92b18-205">Linux</span><span class="sxs-lookup"><span data-stu-id="92b18-205">Linux</span></span> |<span data-ttu-id="92b18-206">Linux, Unix, Mac OS X eller Windows</span><span class="sxs-lookup"><span data-stu-id="92b18-206">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="92b18-207">.NET SDK för Hadoop</span><span class="sxs-lookup"><span data-stu-id="92b18-207">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="92b18-208">✔</span><span class="sxs-lookup"><span data-stu-id="92b18-208">✔</span></span> |<span data-ttu-id="92b18-209">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="92b18-209">Linux or Windows</span></span> |<span data-ttu-id="92b18-210">Windows (för tillfället)</span><span class="sxs-lookup"><span data-stu-id="92b18-210">Windows (for now)</span></span> |
| [<span data-ttu-id="92b18-211">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="92b18-211">Azure PowerShell</span></span>](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |<span data-ttu-id="92b18-212">✔</span><span class="sxs-lookup"><span data-stu-id="92b18-212">✔</span></span> |<span data-ttu-id="92b18-213">Linux- eller Windows</span><span class="sxs-lookup"><span data-stu-id="92b18-213">Linux or Windows</span></span> |<span data-ttu-id="92b18-214">Windows</span><span class="sxs-lookup"><span data-stu-id="92b18-214">Windows</span></span> |

## <a name="limitations"></a><span data-ttu-id="92b18-215">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="92b18-215">Limitations</span></span>
* <span data-ttu-id="92b18-216">Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.</span><span class="sxs-lookup"><span data-stu-id="92b18-216">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="92b18-217">Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning hello infogningsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="92b18-217">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92b18-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92b18-218">Next steps</span></span>
<span data-ttu-id="92b18-219">Nu har du fått veta hur toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="92b18-219">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="92b18-220">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="92b18-220">toolearn more, see:</span></span>

* [<span data-ttu-id="92b18-221">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="92b18-221">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="92b18-222">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="92b18-222">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* <span data-ttu-id="92b18-223">[Använda Oozie med HDInsight][hdinsight-use-oozie]: Använd Sqoop åtgärd i ett arbetsflöde för Oozie.</span><span class="sxs-lookup"><span data-stu-id="92b18-223">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="92b18-224">[Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]: använda Hive tooanalyze svarta fördröjning data och sedan använda Sqoop tooexport data tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-224">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="92b18-225">[Ladda upp data tooHDInsight][hdinsight-upload-data]: hitta andra metoder för att överföra data tooHDInsight/Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="92b18-225">[Upload data tooHDInsight][hdinsight-upload-data]: Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

## <a name="appendix-a---a-powershell-sample"></a><span data-ttu-id="92b18-226">Bilaga A – ett PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="92b18-226">Appendix A - a PowerShell sample</span></span>
<span data-ttu-id="92b18-227">hello PowerShell-exempel utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="92b18-227">hello PowerShell sample performs hello following steps:</span></span>

1. <span data-ttu-id="92b18-228">Ansluta tooAzure.</span><span class="sxs-lookup"><span data-stu-id="92b18-228">Connect tooAzure.</span></span>
2. <span data-ttu-id="92b18-229">Skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="92b18-229">Create an Azure resource group.</span></span> <span data-ttu-id="92b18-230">Mer information finns i [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="92b18-230">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md)</span></span>
3. <span data-ttu-id="92b18-231">Skapa en Azure SQL Database-server, en Azure SQL database och två tabeller.</span><span class="sxs-lookup"><span data-stu-id="92b18-231">Create an Azure SQL Database server, an Azure SQL database, and two tables.</span></span> 
   
    <span data-ttu-id="92b18-232">Om du använder SQL Server i stället använda hello följande instruktioner toocreate hello tabeller:</span><span class="sxs-lookup"><span data-stu-id="92b18-232">If you use SQL Server instead, use hello following statements toocreate hello tables:</span></span>
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    <span data-ttu-id="92b18-233">hello enklaste sättet tooexamine hello-databasen och tabeller är toouse Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92b18-233">hello easiest way tooexamine hello database and tables is toouse Visual Studio.</span></span> <span data-ttu-id="92b18-234">hello-databasservern och hello databasen kan granskas med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="92b18-234">hello database server and hello database can be examined using hello Azure portal.</span></span>
4. <span data-ttu-id="92b18-235">Skapa ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="92b18-235">Create an HDInsight cluster.</span></span>
   
    <span data-ttu-id="92b18-236">tooexamine hello klustret som du kan använda hello Azure-portalen eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92b18-236">tooexamine hello cluster, you can use hello Azure portal or Azure PowerShell.</span></span>
5. <span data-ttu-id="92b18-237">Förbearbeta hello källdatafilen.</span><span class="sxs-lookup"><span data-stu-id="92b18-237">Pre-process hello source data file.</span></span>
   
    <span data-ttu-id="92b18-238">I kursen får exportera du en log4j loggfil (en avgränsad fil) och en Hive tabell tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-238">In this tutorial, you export a log4j log file (a delimited file) and a Hive table tooan Azure SQL database.</span></span> <span data-ttu-id="92b18-239">hello avgränsad fil som kallas */example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="92b18-239">hello delimited file is called */example/data/sample.log*.</span></span> <span data-ttu-id="92b18-240">Tidigare i självstudierna hello såg några exempel på log4j loggar.</span><span class="sxs-lookup"><span data-stu-id="92b18-240">Earlier in hello tutorial, you saw a few samples of log4j logs.</span></span> <span data-ttu-id="92b18-241">Hello loggfil, det inte finns några tomma rader och vissa liknande toothese rader:</span><span class="sxs-lookup"><span data-stu-id="92b18-241">In hello log file, there are some empty lines and some lines similar toothese:</span></span>
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    <span data-ttu-id="92b18-242">Det här är bra för andra exempel som använder dessa data, men vi måste ta bort de här undantagen innan vi kan importera till hello Azure SQL database eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="92b18-242">This is fine for other examples that use this data, but we must remove these exceptions before we can import into hello Azure SQL database or SQL Server.</span></span> <span data-ttu-id="92b18-243">Sqoop export misslyckas om det finns en tom sträng eller en rad med ett mindre element än hello antalet fält som definierats i hello Azure SQL database-tabellen.</span><span class="sxs-lookup"><span data-stu-id="92b18-243">Sqoop export  fails if there is an empty string or a line with a fewer elements than hello number of fields defined in hello Azure SQL database table.</span></span> <span data-ttu-id="92b18-244">Hej log4jlogs tabellen har 7 fält av typen string.</span><span class="sxs-lookup"><span data-stu-id="92b18-244">hello log4jlogs table has 7 string-type fields.</span></span>
   
    <span data-ttu-id="92b18-245">Den här proceduren skapar en ny fil i hello kluster: tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="92b18-245">This procedure creates a new file on hello cluster: tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="92b18-246">tooexamine hello ändrade datafil som du kan använda hello Azure-portalen, ett explorer verktyg för Azure Storage eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92b18-246">tooexamine hello modified data file, you can use hello Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span> <span data-ttu-id="92b18-247">[Komma igång med HDInsight] [ hdinsight-get-started] har en kod som exempel för att använda Azure PowerShell toodownload en fil och visa hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="92b18-247">[Get started with HDInsight][hdinsight-get-started] has a code sample for using Azure PowerShell toodownload a file and display hello file content.</span></span>
6. <span data-ttu-id="92b18-248">Exportera en data-filen toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-248">Export a data file toohello Azure SQL database.</span></span>
   
    <span data-ttu-id="92b18-249">hello källfilen är tutorials/usesqoop/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="92b18-249">hello source file is tutorials/usesqoop/data/sample.log.</span></span> <span data-ttu-id="92b18-250">hello tabellen där hello data är exporterade toois kallas log4jlogs.</span><span class="sxs-lookup"><span data-stu-id="92b18-250">hello table where hello data is exported toois called log4jlogs.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="92b18-251">Förutom informationen i anslutningssträngen, bör hello stegen i det här avsnittet fungera för en Azure SQL database eller SQL Server.</span><span class="sxs-lookup"><span data-stu-id="92b18-251">Other than connection string information, hello steps in this section should work for an Azure SQL database or for SQL Server.</span></span> <span data-ttu-id="92b18-252">Dessa steg har testats med hjälp av hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="92b18-252">These steps were tested by using hello following configuration:</span></span>
   > 
   > * <span data-ttu-id="92b18-253">**Virtuella Azure-nätverket punkt-till-plats configuration**: ett virtuellt nätverk anslutet hello HDInsight-kluster tooa SQL Server i ett privat datacenter.</span><span class="sxs-lookup"><span data-stu-id="92b18-253">**Azure virtual network point-to-site configuration**: A virtual network connected hello HDInsight cluster tooa SQL Server in a private datacenter.</span></span> <span data-ttu-id="92b18-254">Se [konfigurerar en punkt-till-plats-VPN i hello hanteringsportalen](../vpn-gateway/vpn-gateway-point-to-site-create.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="92b18-254">See [Configure a Point-to-Site VPN in hello Management Portal](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>
   > * <span data-ttu-id="92b18-255">**Azure HDInsight 3.1**: se [skapa Hadoop-kluster i HDInsight med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md) information om hur du skapar ett kluster på ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="92b18-255">**Azure HDInsight 3.1**: See [Create Hadoop clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster on a virtual network.</span></span>
   > * <span data-ttu-id="92b18-256">**SQL Server 2014**: konfigurerad tooallow autentisering och körs hello VPN-klienten configuration paketet tooconnect på ett säkert sätt toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="92b18-256">**SQL Server 2014**: Configured tooallow authentication and running hello VPN client configuration package tooconnect securely toohello virtual network.</span></span>
   > 
   > 
7. <span data-ttu-id="92b18-257">Exportera en Hive tabell toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="92b18-257">Export a Hive table toohello Azure SQL database.</span></span>
8. <span data-ttu-id="92b18-258">Importera hello mobiledata tabell toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="92b18-258">Import hello mobiledata table toohello HDInsight cluster.</span></span>
   
    <span data-ttu-id="92b18-259">tooexamine hello ändrade datafil som du kan använda hello Azure-portalen, ett explorer verktyg för Azure Storage eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92b18-259">tooexamine hello modified data file, you can use hello Azure portal, an Azure Storage explorer tool, or Azure PowerShell.</span></span>  <span data-ttu-id="92b18-260">[Komma igång med HDInsight] [ hdinsight-get-started] har en kod exempel om hur du använder Azure PowerShell toodownload en fil och visa hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="92b18-260">[Get started with HDInsight][hdinsight-get-started] has a code sample about using Azure PowerShell toodownload a file and display hello file content.</span></span>

### <a name="hello-powershell-sample"></a><span data-ttu-id="92b18-261">hello PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="92b18-261">hello PowerShell sample</span></span>
    # Prepare an Azure SQL database toobe used by hello Sqoop tutorial

    #region - provide hello following values

    $subscriptionID = "<Enter your Azure Subscription ID>"

    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"

    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"

    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"

    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"

    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region - Create tables
    Write-Host "Creating hello log4jlogs table and hello mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create hello mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process hello source file

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define hello connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing hello source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from hello source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into hello destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process hello source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove hello "java.lang.Exception" from hello first element of hello array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList tooremove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove hello lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write toohello destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from hello cluster toohello SQL database

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    $tableName_log4j = "log4jlogs"

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $exportDir_log4j = "/tutorials/usesqoop/data"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

    #endregion

    #region - export a Hive table

    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion

    #region - import a database

    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
