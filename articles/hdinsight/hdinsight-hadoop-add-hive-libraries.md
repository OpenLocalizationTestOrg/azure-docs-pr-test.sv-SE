---
title: "Skapa - Azure för aaaAdd Hive-bibliotek under HDInsight-kluster | Microsoft Docs"
description: "Lär dig hur tooadd Hive-bibliotek (jar-filer), tooan HDInsight-kluster när klustret skapas."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 2e028a07c3248205def0789af2c262a0774a8f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a><span data-ttu-id="6a84f-103">Lägg till anpassade Hive-bibliotek när du skapar ditt HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="6a84f-103">Add custom Hive libraries when creating your HDInsight cluster</span></span>

<span data-ttu-id="6a84f-104">Om du har bibliotek som du använder ofta med Hive i HDInsight kan innehåller det här dokumentet information om hur du använder en skriptåtgärd toopre belastningen hello bibliotek när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="6a84f-104">If you have libraries that you use frequently with Hive on HDInsight, this document contains information on using a Script Action toopre-load hello libraries during cluster creation.</span></span> <span data-ttu-id="6a84f-105">Bibliotek som lagts till med hello stegen i det här dokumentet är globalt tillgänglig i Hive - det finns inget behov av toouse [JAR-Lägg till](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload dem.</span><span class="sxs-lookup"><span data-stu-id="6a84f-105">Libraries added using hello steps in this document are globally available in Hive - there is no need toouse [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload them.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="6a84f-106">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="6a84f-106">How it works</span></span>

<span data-ttu-id="6a84f-107">Du kan också ange en skriptåtgärd som kör ett skript på klusternoder hello medan de skapas när du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="6a84f-107">When creating a cluster, you can optionally specify a Script Action that runs a script on hello cluster nodes while they are being created.</span></span> <span data-ttu-id="6a84f-108">hello skript i det här dokumentet accepterar en enda parameter som är en WASB-plats som innehåller hello bibliotek (som lagras som jar-filer) toobe redan har lästs in.</span><span class="sxs-lookup"><span data-stu-id="6a84f-108">hello script in this document accepts a single parameter, which is a WASB location that contains hello libraries (stored as jar files) toobe pre-loaded.</span></span>

<span data-ttu-id="6a84f-109">När klustret skapas hello skript räknar hello filer, kopierar dem toohello `/usr/lib/customhivelibs/` katalog på head- och arbetsroller noder och lägger sedan till dem toohello `hive.aux.jars.path` egenskap i hello `core-site.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="6a84f-109">During cluster creation, hello script enumerates hello files, copies them toohello `/usr/lib/customhivelibs/` directory on head and worker nodes, then adds them toohello `hive.aux.jars.path` property in hello `core-site.xml` file.</span></span> <span data-ttu-id="6a84f-110">På Linux-baserade kluster även uppdateras hello `hive-env.sh` fil med hello var hello-filer.</span><span class="sxs-lookup"><span data-stu-id="6a84f-110">On Linux-based clusters, it also updates hello `hive-env.sh` file with hello location of hello files.</span></span>

> [!NOTE]
> <span data-ttu-id="6a84f-111">Med hjälp av hello skriptåtgärder i den här artikeln tillgängliggör hello bibliotek i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="6a84f-111">Using hello script actions in this article makes hello libraries available in hello following scenarios:</span></span>
>
> * <span data-ttu-id="6a84f-112">**Linux-baserat HDInsight** – när du använder hello en Hive-klient **WebHCat**, och **HiveServer2**.</span><span class="sxs-lookup"><span data-stu-id="6a84f-112">**Linux-based HDInsight** - when using hello a Hive client, **WebHCat**, and **HiveServer2**.</span></span>
> * <span data-ttu-id="6a84f-113">**Windows-baserade HDInsight** – när hello Hive-klienten och **WebHCat**.</span><span class="sxs-lookup"><span data-stu-id="6a84f-113">**Windows-based HDInsight** - when using hello Hive client and **WebHCat**.</span></span>

## <a name="hello-script"></a><span data-ttu-id="6a84f-114">hello-skript</span><span class="sxs-lookup"><span data-stu-id="6a84f-114">hello script</span></span>

<span data-ttu-id="6a84f-115">**Skriptets placering**</span><span class="sxs-lookup"><span data-stu-id="6a84f-115">**Script location**</span></span>

<span data-ttu-id="6a84f-116">För **Linux-baserade kluster**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="6a84f-116">For **Linux-based clusters**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span></span>

<span data-ttu-id="6a84f-117">För **Windows-baserade kluster**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span><span class="sxs-lookup"><span data-stu-id="6a84f-117">For **Windows-based clusters**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a84f-118">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6a84f-118">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6a84f-119">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6a84f-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="6a84f-120">**Krav**</span><span class="sxs-lookup"><span data-stu-id="6a84f-120">**Requirements**</span></span>

* <span data-ttu-id="6a84f-121">hello skript måste vara tillämpade tooboth hello **huvudnoder** och **arbetarnoder**.</span><span class="sxs-lookup"><span data-stu-id="6a84f-121">hello scripts must be applied tooboth hello **Head nodes** and **Worker nodes**.</span></span>

* <span data-ttu-id="6a84f-122">Hej burkar gärna tooinstall måste lagras i Azure Blob Storage i en **enskild behållare**.</span><span class="sxs-lookup"><span data-stu-id="6a84f-122">hello jars you wish tooinstall must be stored in Azure Blob Storage in a **single container**.</span></span>

* <span data-ttu-id="6a84f-123">hello storage-konto som innehåller hello bibliotek med jar-filer **måste** vara länkade toohello HDInsight-kluster när du skapar.</span><span class="sxs-lookup"><span data-stu-id="6a84f-123">hello storage account containing hello library of jar files **must** be linked toohello HDInsight cluster during creation.</span></span> <span data-ttu-id="6a84f-124">Det måste antingen vara hello standardkontot för lagring eller ett konto som har lagts till via __valfri konfiguration__.</span><span class="sxs-lookup"><span data-stu-id="6a84f-124">It must either be hello default storage account, or an account added through __optional configuration__.</span></span>

* <span data-ttu-id="6a84f-125">Hej WASB sökväg toohello behållaren måste anges som en parameter toohello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6a84f-125">hello WASB path toohello container must be specified as a parameter toohello Script Action.</span></span> <span data-ttu-id="6a84f-126">Till exempel om hello burkar lagras i en behållare med namnet **libs** på ett lagringskonto med namnet **mystorage**, hello parametern skulle vara  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="6a84f-126">For example, if hello jars are stored in a container named **libs** on a storage account named **mystorage**, hello parameter would be **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6a84f-127">Det här dokumentet förutsätts att du redan har skapar ett lagringskonto, blob-behållaren och överförda hello filer tooit.</span><span class="sxs-lookup"><span data-stu-id="6a84f-127">This document assumes that you have already create a storage account, blob container, and uploaded hello files tooit.</span></span>
  >
  > <span data-ttu-id="6a84f-128">Om du inte har skapat ett lagringskonto kan du göra det via hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a84f-128">If you have not created a storage account, you can do so through hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6a84f-129">Du kan sedan använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/) toocreate en behållare i hello-konto och ladda upp filer tooit.</span><span class="sxs-lookup"><span data-stu-id="6a84f-129">You can then use a utility such as [Azure Storage Explorer](http://storageexplorer.com/) toocreate a container in hello account and upload files tooit.</span></span>

## <a name="create-a-cluster-using-hello-script"></a><span data-ttu-id="6a84f-130">Skapa ett kluster med hello skript</span><span class="sxs-lookup"><span data-stu-id="6a84f-130">Create a cluster using hello script</span></span>

> [!NOTE]
> <span data-ttu-id="6a84f-131">hello följande skapa ett Linux-baserat HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6a84f-131">hello following steps create a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="6a84f-132">toocreate ett Windows-baserade kluster, Välj **Windows** som hello kluster-OS när du skapar hello kluster och använda hello Windows (PowerShell) skript i stället för hello bash-skript.</span><span class="sxs-lookup"><span data-stu-id="6a84f-132">toocreate a Windows-based cluster, select **Windows** as hello cluster OS when creating hello cluster, and use hello Windows (PowerShell) script instead of hello bash script.</span></span>
>
> <span data-ttu-id="6a84f-133">Du kan också använda Azure PowerShell eller hello HDInsight .NET SDK toocreate ett kluster med det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="6a84f-133">You can also use Azure PowerShell or hello HDInsight .NET SDK toocreate a cluster using this script.</span></span> <span data-ttu-id="6a84f-134">Mer information om hur du använder metoderna finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6a84f-134">For more information on using these methods, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="6a84f-135">Börja etablera ett kluster med hjälp av hello stegen i [etablera HDInsight-kluster på Linux](hdinsight-hadoop-provision-linux-clusters.md), men inte slutföra etablering.</span><span class="sxs-lookup"><span data-stu-id="6a84f-135">Start provisioning a cluster by using hello steps in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md), but do not complete provisioning.</span></span>

2. <span data-ttu-id="6a84f-136">På hello **valfri konfiguration** bladet väljer **skriptåtgärder**, och ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="6a84f-136">On hello **Optional Configuration** blade, select **Script Actions**, and provide hello following information:</span></span>

   * <span data-ttu-id="6a84f-137">**NAMNET**: Ange ett eget namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6a84f-137">**NAME**: Enter a friendly name for hello script action.</span></span>

   * <span data-ttu-id="6a84f-138">**SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span><span class="sxs-lookup"><span data-stu-id="6a84f-138">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span></span>

   * <span data-ttu-id="6a84f-139">**HEAD**: Markera det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="6a84f-139">**HEAD**: Check this option.</span></span>

   * <span data-ttu-id="6a84f-140">**WORKER**: Markera det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="6a84f-140">**WORKER**: Check this option.</span></span>

   * <span data-ttu-id="6a84f-141">**ZOOKEEPER**: lämnar fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="6a84f-141">**ZOOKEEPER**: Leave this blank.</span></span>

   * <span data-ttu-id="6a84f-142">**PARAMETRARNA**: Ange hello WASB adress toohello behållare och storage-konto som innehåller hello burkar.</span><span class="sxs-lookup"><span data-stu-id="6a84f-142">**PARAMETERS**: Enter hello WASB address toohello container and storage account that contains hello jars.</span></span> <span data-ttu-id="6a84f-143">Till exempel  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="6a84f-143">For example, **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

3. <span data-ttu-id="6a84f-144">Längst ned hello hello **skriptåtgärder**, använda hello **Välj** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6a84f-144">At hello bottom of hello **Script Actions**, use hello **Select** button toosave hello configuration.</span></span>

4. <span data-ttu-id="6a84f-145">På hello **valfri konfiguration** bladet väljer **länkade Lagringskonton** och välj hello **lägga till en nyckel för säkerhetslagring** länk.</span><span class="sxs-lookup"><span data-stu-id="6a84f-145">On hello **Optional Configuration** blade, select **Linked Storage Accounts** and select hello **Add a storage key** link.</span></span> <span data-ttu-id="6a84f-146">Välj hello storage-konto som innehåller hello burkar och Använd sedan hello **Välj** knappar toosave inställningar och returnera hello **valfri konfiguration** bladet.</span><span class="sxs-lookup"><span data-stu-id="6a84f-146">Select hello storage account that contains hello jars, and then use hello **select** buttons toosave settings and return hello **Optional Configuration** blade.</span></span>

5. <span data-ttu-id="6a84f-147">Använd hello **Välj** knappen längst ned hello hello **valfri konfiguration** bladet toosave hello ytterligare konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="6a84f-147">Use hello **Select** button at hello bottom of hello **Optional Configuration** blade toosave hello optional configuration information.</span></span>

6. <span data-ttu-id="6a84f-148">Fortsätta etablering hello klustret enligt beskrivningen i [etablera HDInsight-kluster på Linux](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="6a84f-148">Continue provisioning hello cluster as described in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="6a84f-149">När klustret skapas är klar är du kan toouse hello burkar lagts till via det här skriptet från Hive utan toouse hello `ADD JAR` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6a84f-149">Once cluster creation finishes, you are able toouse hello jars added through this script from Hive without having toouse hello `ADD JAR` statement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a84f-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a84f-150">Next steps</span></span>

<span data-ttu-id="6a84f-151">Mer information om hur du arbetar med Hive finns [använda Hive med HDInsight](hdinsight-use-hive.md)</span><span class="sxs-lookup"><span data-stu-id="6a84f-151">For more information on working with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md)</span></span>
