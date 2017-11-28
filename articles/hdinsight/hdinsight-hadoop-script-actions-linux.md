---
title: aaaScript utveckling med Linux-baserade HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse Bash skript toocustomize Linux-baserade HDInsight-kluster. hello skript åtgärd funktion i HDInsight kan du toorun skript under eller efter att klustret har skapats. Skript kan använda toochange inställningar för klustrets eller installera ytterligare programvara."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 1f504b00365df5f4cfb3ae19ad55ff7630342650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="script-action-development-with-hdinsight"></a><span data-ttu-id="424d7-105">Skriptutveckling med HDInsight</span><span class="sxs-lookup"><span data-stu-id="424d7-105">Script action development with HDInsight</span></span>

<span data-ttu-id="424d7-106">Lär dig hur toocustomize ditt HDInsight-kluster med Bash skript.</span><span class="sxs-lookup"><span data-stu-id="424d7-106">Learn how toocustomize your HDInsight cluster using Bash scripts.</span></span> <span data-ttu-id="424d7-107">Skriptåtgärder är ett sätt toocustomize HDInsight under eller efter att klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="424d7-107">Script actions are a way toocustomize HDInsight during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="424d7-108">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="424d7-108">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="424d7-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="424d7-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="424d7-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="424d7-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="what-are-script-actions"></a><span data-ttu-id="424d7-111">Vad är skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="424d7-111">What are script actions</span></span>

<span data-ttu-id="424d7-112">Skriptåtgärder Bash-skript som Azure körs på hello ändringar i klusterkonfigurationen noder toomake eller installera programvara.</span><span class="sxs-lookup"><span data-stu-id="424d7-112">Script actions are Bash scripts that Azure runs on hello cluster nodes toomake configuration changes or install software.</span></span> <span data-ttu-id="424d7-113">En skriptåtgärd körs som rot och ger fullständig åtkomst rättigheter toohello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="424d7-113">A script action is executed as root, and provides full access rights toohello cluster nodes.</span></span>

<span data-ttu-id="424d7-114">Skriptåtgärder kan tillämpas via hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="424d7-114">Script actions can be applied through hello following methods:</span></span>

| <span data-ttu-id="424d7-115">Använd den här metoden tooapply skript...</span><span class="sxs-lookup"><span data-stu-id="424d7-115">Use this method tooapply a script...</span></span> | <span data-ttu-id="424d7-116">Skapa kluster under...</span><span class="sxs-lookup"><span data-stu-id="424d7-116">During cluster creation...</span></span> | <span data-ttu-id="424d7-117">På ett kluster som körs...</span><span class="sxs-lookup"><span data-stu-id="424d7-117">On a running cluster...</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="424d7-118">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="424d7-118">Azure portal</span></span> |<span data-ttu-id="424d7-119">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-119">✓</span></span> |<span data-ttu-id="424d7-120">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-120">✓</span></span> |
| <span data-ttu-id="424d7-121">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="424d7-121">Azure PowerShell</span></span> |<span data-ttu-id="424d7-122">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-122">✓</span></span> |<span data-ttu-id="424d7-123">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-123">✓</span></span> |
| <span data-ttu-id="424d7-124">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="424d7-124">Azure CLI</span></span> |&nbsp; |<span data-ttu-id="424d7-125">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-125">✓</span></span> |
| <span data-ttu-id="424d7-126">HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="424d7-126">HDInsight .NET SDK</span></span> |<span data-ttu-id="424d7-127">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-127">✓</span></span> |<span data-ttu-id="424d7-128">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-128">✓</span></span> |
| <span data-ttu-id="424d7-129">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="424d7-129">Azure Resource Manager Template</span></span> |<span data-ttu-id="424d7-130">✓</span><span class="sxs-lookup"><span data-stu-id="424d7-130">✓</span></span> |&nbsp; |

<span data-ttu-id="424d7-131">Mer information om hur du använder dessa metoder tooapply skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="424d7-131">For more information on using these methods tooapply script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="424d7-132"><a name="bestPracticeScripting"></a>Metodtips för skriptutveckling av</span><span class="sxs-lookup"><span data-stu-id="424d7-132"><a name="bestPracticeScripting"></a>Best practices for script development</span></span>

<span data-ttu-id="424d7-133">När du utvecklar ett anpassat skript för ett HDInsight-kluster finns flera bästa praxis tookeep i åtanke:</span><span class="sxs-lookup"><span data-stu-id="424d7-133">When you develop a custom script for an HDInsight cluster, there are several best practices tookeep in mind:</span></span>

* [<span data-ttu-id="424d7-134">Målversionen hello Hadoop</span><span class="sxs-lookup"><span data-stu-id="424d7-134">Target hello Hadoop version</span></span>](#bPS1)
* [<span data-ttu-id="424d7-135">Mål hello OS-Version</span><span class="sxs-lookup"><span data-stu-id="424d7-135">Target hello OS Version</span></span>](#bps10)
* [<span data-ttu-id="424d7-136">Ange stabil länkar tooscript resurser</span><span class="sxs-lookup"><span data-stu-id="424d7-136">Provide stable links tooscript resources</span></span>](#bPS2)
* [<span data-ttu-id="424d7-137">Använda fördefinierade kompilerade resurser</span><span class="sxs-lookup"><span data-stu-id="424d7-137">Use pre-compiled resources</span></span>](#bPS4)
* [<span data-ttu-id="424d7-138">Se till att hello kluster anpassning skriptet idempotent</span><span class="sxs-lookup"><span data-stu-id="424d7-138">Ensure that hello cluster customization script is idempotent</span></span>](#bPS3)
* [<span data-ttu-id="424d7-139">Garantera hög tillgänglighet för hello kluster-arkitektur</span><span class="sxs-lookup"><span data-stu-id="424d7-139">Ensure high availability of hello cluster architecture</span></span>](#bPS5)
* [<span data-ttu-id="424d7-140">Konfigurera hello anpassade komponenter toouse Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="424d7-140">Configure hello custom components toouse Azure Blob storage</span></span>](#bPS6)
* [<span data-ttu-id="424d7-141">Skriva information tooSTDOUT och STDERR</span><span class="sxs-lookup"><span data-stu-id="424d7-141">Write information tooSTDOUT and STDERR</span></span>](#bPS7)
* [<span data-ttu-id="424d7-142">Spara filer i ASCII-format med LF radbrytningar</span><span class="sxs-lookup"><span data-stu-id="424d7-142">Save files as ASCII with LF line endings</span></span>](#bps8)
* [<span data-ttu-id="424d7-143">Använd försök logik toorecover från tillfälliga fel</span><span class="sxs-lookup"><span data-stu-id="424d7-143">Use retry logic toorecover from transient errors</span></span>](#bps9)

> [!IMPORTANT]
> <span data-ttu-id="424d7-144">Skriptåtgärder måste slutföras inom 60 minuter eller hello misslyckas.</span><span class="sxs-lookup"><span data-stu-id="424d7-144">Script actions must complete within 60 minutes or hello process fails.</span></span> <span data-ttu-id="424d7-145">Under noden etableringen körs hello skriptet samtidigt med andra processer för installation och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="424d7-145">During node provisioning, hello script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="424d7-146">Konkurrens om resurser, till exempel CPU-tid eller nätverket bandbredd kan orsaka hello skriptet tootake längre toofinish än i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="424d7-146">Competition for resources such as CPU time or network bandwidth may cause hello script tootake longer toofinish than it does in your development environment.</span></span>

### <span data-ttu-id="424d7-147"><a name="bPS1"></a>Målversionen hello Hadoop</span><span class="sxs-lookup"><span data-stu-id="424d7-147"><a name="bPS1"></a>Target hello Hadoop version</span></span>

<span data-ttu-id="424d7-148">Olika versioner av HDInsight har olika versioner av Hadoop-tjänster och komponenter installeras.</span><span class="sxs-lookup"><span data-stu-id="424d7-148">Different versions of HDInsight have different versions of Hadoop services and components installed.</span></span> <span data-ttu-id="424d7-149">Om skriptet förväntar en viss version av en tjänst eller en komponent, bör du bara använda hello skript med hello version av HDInsight som innehåller hello nödvändiga komponenter.</span><span class="sxs-lookup"><span data-stu-id="424d7-149">If your script expects a specific version of a service or component, you should only use hello script with hello version of HDInsight that includes hello required components.</span></span> <span data-ttu-id="424d7-150">Du hittar information om komponenten-versioner som ingår i HDInsight med hjälp av hello [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="424d7-150">You can find information on component versions included with HDInsight using hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

### <span data-ttu-id="424d7-151"><a name="bps10"></a>Målversionen hello OS</span><span class="sxs-lookup"><span data-stu-id="424d7-151"><a name="bps10"></a> Target hello OS version</span></span>

<span data-ttu-id="424d7-152">Linux-baserat HDInsight baseras på hello Ubuntu Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="424d7-152">Linux-based HDInsight is based on hello Ubuntu Linux distribution.</span></span> <span data-ttu-id="424d7-153">Olika versioner av HDInsight förlitar sig på olika versioner av Ubuntu som kan ändra hur skriptet fungerar.</span><span class="sxs-lookup"><span data-stu-id="424d7-153">Different versions of HDInsight rely on different versions of Ubuntu, which may change how your script behaves.</span></span> <span data-ttu-id="424d7-154">Till exempel HDInsight 3,4 och tidigare baseras på Ubuntu-versioner som använder Upstart.</span><span class="sxs-lookup"><span data-stu-id="424d7-154">For example, HDInsight 3.4 and earlier are based on Ubuntu versions that use Upstart.</span></span> <span data-ttu-id="424d7-155">Version 3.5 är baserad på Ubuntu 16.04 som använder Systemd.</span><span class="sxs-lookup"><span data-stu-id="424d7-155">Version 3.5 is based on Ubuntu 16.04, which uses Systemd.</span></span> <span data-ttu-id="424d7-156">Systemd och Upstart förlitar sig på olika kommandon så att skriptet ska skrivas toowork med båda.</span><span class="sxs-lookup"><span data-stu-id="424d7-156">Systemd and Upstart rely on different commands, so your script should be written toowork with both.</span></span>

<span data-ttu-id="424d7-157">En annan viktig skillnad mellan HDInsight 3.4 och 3.5 är att `JAVA_HOME` pekar nu tooJava 8.</span><span class="sxs-lookup"><span data-stu-id="424d7-157">Another important difference between HDInsight 3.4 and 3.5 is that `JAVA_HOME` now points tooJava 8.</span></span>

<span data-ttu-id="424d7-158">Du kan kontrollera hello OS-version med hjälp av `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="424d7-158">You can check hello OS version by using `lsb_release`.</span></span> <span data-ttu-id="424d7-159">hello följande kod visar hur toodetermine om hello skript körs på Ubuntu 14 eller 16:</span><span class="sxs-lookup"><span data-stu-id="424d7-159">hello following code demonstrates how toodetermine if hello script is running on Ubuntu 14 or 16:</span></span>

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

<span data-ttu-id="424d7-160">Du kan hitta hello fullständig skript som innehåller dessa kodavsnitt på https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span><span class="sxs-lookup"><span data-stu-id="424d7-160">You can find hello full script that contains these snippets at https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span></span>

<span data-ttu-id="424d7-161">Hello version av Ubuntu som används av HDInsight finns hello [HDInsight komponentversionen](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="424d7-161">For hello version of Ubuntu that is used by HDInsight, see hello [HDInsight component version](hdinsight-component-versioning.md) document.</span></span>

<span data-ttu-id="424d7-162">toounderstand hello skillnader mellan Systemd och Upstart, se [Systemd för Upstart användare](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span><span class="sxs-lookup"><span data-stu-id="424d7-162">toounderstand hello differences between Systemd and Upstart, see [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span></span>

### <span data-ttu-id="424d7-163"><a name="bPS2"></a>Ange stabil länkar tooscript resurser</span><span class="sxs-lookup"><span data-stu-id="424d7-163"><a name="bPS2"></a>Provide stable links tooscript resources</span></span>

<span data-ttu-id="424d7-164">hello måste skript och associerade resurser vara tillgängliga i hela hello livstid hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="424d7-164">hello script and associated resources must remain available throughout hello lifetime of hello cluster.</span></span> <span data-ttu-id="424d7-165">Dessa resurser krävs om nya noder läggs toohello kluster under skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="424d7-165">These resources are required if new nodes are added toohello cluster during scaling operations.</span></span>

<span data-ttu-id="424d7-166">hello bästa praxis är toodownload och arkivera allt innehåll i ett Azure Storage-konto på din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="424d7-166">hello best practice is toodownload and archive everything in an Azure Storage account on your subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="424d7-167">hello storage-konto som används måste vara hello standardkontot för lagring för hello kluster eller en offentlig, skrivskyddad behållare för andra storage-konto.</span><span class="sxs-lookup"><span data-stu-id="424d7-167">hello storage account used must be hello default storage account for hello cluster or a public, read-only container on any other storage account.</span></span>

<span data-ttu-id="424d7-168">Till exempel hello-exempel som tillhandahålls av Microsoft lagras i hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage-konto.</span><span class="sxs-lookup"><span data-stu-id="424d7-168">For example, hello samples provided by Microsoft are stored in hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage account.</span></span> <span data-ttu-id="424d7-169">Detta är en offentlig, skrivskyddad behållare som underhålls av hello HDInsight-teamet.</span><span class="sxs-lookup"><span data-stu-id="424d7-169">This is a public, read-only container maintained by hello HDInsight team.</span></span>

### <span data-ttu-id="424d7-170"><a name="bPS4"></a>Använda fördefinierade kompilerade resurser</span><span class="sxs-lookup"><span data-stu-id="424d7-170"><a name="bPS4"></a>Use pre-compiled resources</span></span>

<span data-ttu-id="424d7-171">tooreduce hello tid det tar toorun hello skript, undvika åtgärder som sammanställer resurser från källkoden.</span><span class="sxs-lookup"><span data-stu-id="424d7-171">tooreduce hello time it takes toorun hello script, avoid operations that compile resources from source code.</span></span> <span data-ttu-id="424d7-172">Till exempel före kompilera resurser och lagrar dem i en blob med Azure Storage-konto i hello samma datacenter som HDInsight.</span><span class="sxs-lookup"><span data-stu-id="424d7-172">For example, pre-compile resources and store them in an Azure Storage account blob in hello same data center as HDInsight.</span></span>

### <span data-ttu-id="424d7-173"><a name="bPS3"></a>Se till att hello kluster anpassning skriptet idempotent</span><span class="sxs-lookup"><span data-stu-id="424d7-173"><a name="bPS3"></a>Ensure that hello cluster customization script is idempotent</span></span>

<span data-ttu-id="424d7-174">Skript måste vara idempotent.</span><span class="sxs-lookup"><span data-stu-id="424d7-174">Scripts must be idempotent.</span></span> <span data-ttu-id="424d7-175">Om hello skriptet körs flera gånger, som det ska returnera hello klustret toohello samma tillstånd varje gång.</span><span class="sxs-lookup"><span data-stu-id="424d7-175">If hello script runs multiple times, it should return hello cluster toohello same state every time.</span></span>

<span data-ttu-id="424d7-176">Till exempel ett skript som ändrar konfigurationsfiler bör inte lägga till dubblettposter om körde flera gånger.</span><span class="sxs-lookup"><span data-stu-id="424d7-176">For example, a script that modifies configuration files should not add duplicate entries if ran multiple times.</span></span>

### <span data-ttu-id="424d7-177"><a name="bPS5"></a>Garantera hög tillgänglighet för hello kluster-arkitektur</span><span class="sxs-lookup"><span data-stu-id="424d7-177"><a name="bPS5"></a>Ensure high availability of hello cluster architecture</span></span>

<span data-ttu-id="424d7-178">Linux-baserade HDInsight-kluster tillhandahåller två huvudnoderna som är aktiva i hello kluster och skriptåtgärder köras på båda noderna.</span><span class="sxs-lookup"><span data-stu-id="424d7-178">Linux-based HDInsight clusters provide two head nodes that are active within hello cluster, and script actions run on both nodes.</span></span> <span data-ttu-id="424d7-179">Om hello komponenter du installerar förväntas bara en huvudnod kan du inte installera hello-komponenter på båda huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="424d7-179">If hello components you install expect only one head node, do not install hello components on both head nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="424d7-180">Tjänster som tillhandahålls som en del av HDInsight är utformad toofail över mellan hello två huvudnoderna efter behov.</span><span class="sxs-lookup"><span data-stu-id="424d7-180">Services provided as part of HDInsight are designed toofail over between hello two head nodes as needed.</span></span> <span data-ttu-id="424d7-181">Den här funktionen är inte utökat toocustom komponenter som installeras via skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="424d7-181">This functionality is not extended toocustom components installed through script actions.</span></span> <span data-ttu-id="424d7-182">Om du behöver hög tillgänglighet för anpassade komponenter måste du implementera en egen mekanism för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="424d7-182">If you need high availability for custom components, you must implement your own failover mechanism.</span></span>

### <span data-ttu-id="424d7-183"><a name="bPS6"></a>Konfigurera hello anpassade komponenter toouse Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="424d7-183"><a name="bPS6"></a>Configure hello custom components toouse Azure Blob storage</span></span>

<span data-ttu-id="424d7-184">Komponenter som installeras på hello klustret kan ha en standardkonfiguration som använder Hadoop Distributed File System (HDFS) lagring.</span><span class="sxs-lookup"><span data-stu-id="424d7-184">Components that you install on hello cluster might have a default configuration that uses Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="424d7-185">HDInsight använder antingen Azure Storage eller Azure Data Lake Store som hello standardlagring.</span><span class="sxs-lookup"><span data-stu-id="424d7-185">HDInsight uses either Azure Storage or Data Lake Store as hello default storage.</span></span> <span data-ttu-id="424d7-186">Båda ger ett kompatibelt HDFS-filsystem som kvarstår data även om hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="424d7-186">Both provide an HDFS compatible file system that persists data even if hello cluster is deleted.</span></span> <span data-ttu-id="424d7-187">Du kan behöva tooconfigure komponenter du installerar toouse WASB eller ADL i stället för HDFS.</span><span class="sxs-lookup"><span data-stu-id="424d7-187">You may need tooconfigure components you install toouse WASB or ADL instead of HDFS.</span></span>

<span data-ttu-id="424d7-188">De flesta åtgärder behöver du inte toospecify hello-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="424d7-188">For most operations, you do not need toospecify hello file system.</span></span> <span data-ttu-id="424d7-189">Till exempel kopierar hello följande hello giraph examples.jar fil från hello lokala system toocluster fillagring:</span><span class="sxs-lookup"><span data-stu-id="424d7-189">For example, hello following copies hello giraph-examples.jar file from hello local file system toocluster storage:</span></span>

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

<span data-ttu-id="424d7-190">I det här exemplet hello `hdfs` kommando använder transparent hello standard klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="424d7-190">In this example, hello `hdfs` command transparently uses hello default cluster storage.</span></span> <span data-ttu-id="424d7-191">För vissa åtgärder, kanske du måste toospecify hello URI.</span><span class="sxs-lookup"><span data-stu-id="424d7-191">For some operations, you may need toospecify hello URI.</span></span> <span data-ttu-id="424d7-192">Till exempel `adl:///example/jars` för Data Lake Store eller `wasb:///example/jars` för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="424d7-192">For example, `adl:///example/jars` for Data Lake Store or `wasb:///example/jars` for Azure Storage.</span></span>

### <span data-ttu-id="424d7-193"><a name="bPS7"></a>Skriva information tooSTDOUT och STDERR</span><span class="sxs-lookup"><span data-stu-id="424d7-193"><a name="bPS7"></a>Write information tooSTDOUT and STDERR</span></span>

<span data-ttu-id="424d7-194">HDInsight loggar utdata från skriptet som är skrivna tooSTDOUT och STDERR.</span><span class="sxs-lookup"><span data-stu-id="424d7-194">HDInsight logs script output that is written tooSTDOUT and STDERR.</span></span> <span data-ttu-id="424d7-195">Du kan visa den här informationen med hjälp av hello Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="424d7-195">You can view this information using hello Ambari web UI.</span></span>

> [!NOTE]
> <span data-ttu-id="424d7-196">Ambari är endast tillgänglig om hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="424d7-196">Ambari is only available if hello cluster is successfully created.</span></span> <span data-ttu-id="424d7-197">Om du använder en skriptåtgärd under klustret har skapats och skapa misslyckas, finns i avsnittet om felsökning hello [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) för andra sätt att komma åt loggade informationen.</span><span class="sxs-lookup"><span data-stu-id="424d7-197">If you use a script action during cluster creation, and creation fails, see hello troubleshooting section [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) for other ways of accessing logged information.</span></span>

<span data-ttu-id="424d7-198">De flesta verktyg och Installationspaketen skriva redan information tooSTDOUT och STDERR, men du kanske vill tooadd ytterligare loggning.</span><span class="sxs-lookup"><span data-stu-id="424d7-198">Most utilities and installation packages already write information tooSTDOUT and STDERR, however you may want tooadd additional logging.</span></span> <span data-ttu-id="424d7-199">toosend text tooSTDOUT, Använd `echo`.</span><span class="sxs-lookup"><span data-stu-id="424d7-199">toosend text tooSTDOUT, use `echo`.</span></span> <span data-ttu-id="424d7-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="424d7-200">For example:</span></span>

```bash
echo "Getting ready tooinstall Foo"
```

<span data-ttu-id="424d7-201">Som standard `echo` skickar hello sträng tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="424d7-201">By default, `echo` sends hello string tooSTDOUT.</span></span> <span data-ttu-id="424d7-202">toodirect den tooSTDERR, lägga till `>&2` innan `echo`.</span><span class="sxs-lookup"><span data-stu-id="424d7-202">toodirect it tooSTDERR, add `>&2` before `echo`.</span></span> <span data-ttu-id="424d7-203">Exempel:</span><span class="sxs-lookup"><span data-stu-id="424d7-203">For example:</span></span>

```bash
>&2 echo "An error occurred installing Foo"
```

<span data-ttu-id="424d7-204">Detta omdirigerar information skrivs tooSTDOUT tooSTDERR (2) i stället.</span><span class="sxs-lookup"><span data-stu-id="424d7-204">This redirects information written tooSTDOUT tooSTDERR (2) instead.</span></span> <span data-ttu-id="424d7-205">Mer information om i/o-omdirigering finns [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span><span class="sxs-lookup"><span data-stu-id="424d7-205">For more information on IO redirection, see [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span></span>

<span data-ttu-id="424d7-206">Mer information om hur du visar information som loggas av skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span><span class="sxs-lookup"><span data-stu-id="424d7-206">For more information on viewing information logged by script actions, see [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span></span>

### <span data-ttu-id="424d7-207"><a name="bps8"></a>Spara filer i ASCII-format med LF radbrytningar</span><span class="sxs-lookup"><span data-stu-id="424d7-207"><a name="bps8"></a> Save files as ASCII with LF line endings</span></span>

<span data-ttu-id="424d7-208">Bash-skript ska lagras som ASCII-format med rader som avslutas av LF.</span><span class="sxs-lookup"><span data-stu-id="424d7-208">Bash scripts should be stored as ASCII format, with lines terminated by LF.</span></span> <span data-ttu-id="424d7-209">Filer som lagras som UTF-8 eller använda CRLF som hello rad avslutas kan misslyckas med hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="424d7-209">Files that are stored as UTF-8, or use CRLF as hello line ending may fail with hello following error:</span></span>

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <span data-ttu-id="424d7-210"><a name="bps9"></a>Använd försök logik toorecover från tillfälliga fel</span><span class="sxs-lookup"><span data-stu-id="424d7-210"><a name="bps9"></a> Use retry logic toorecover from transient errors</span></span>

<span data-ttu-id="424d7-211">Vid hämtning av filer, installera paket med lgh get eller andra åtgärder som överför data via Hej internet, hello åtgärden misslyckas på grund av tootransient nätverksfel.</span><span class="sxs-lookup"><span data-stu-id="424d7-211">When downloading files, installing packages using apt-get, or other actions that transmit data over hello internet, hello action may fail due tootransient networking errors.</span></span> <span data-ttu-id="424d7-212">Exempelvis kanske hello fjärresursen du kommunicerar med pågående hello misslyckas över tooa säkerhetskopiering nod.</span><span class="sxs-lookup"><span data-stu-id="424d7-212">For example, hello remote resource you are communicating with may be in hello process of failing over tooa backup node.</span></span>

<span data-ttu-id="424d7-213">toomake din skriptfel flexibel tootransient, du kan implementera logik för omprövning.</span><span class="sxs-lookup"><span data-stu-id="424d7-213">toomake your script resilient tootransient errors, you can implement retry logic.</span></span> <span data-ttu-id="424d7-214">hello följande funktion visar hur tooimplement försök logik.</span><span class="sxs-lookup"><span data-stu-id="424d7-214">hello following function demonstrates how tooimplement retry logic.</span></span> <span data-ttu-id="424d7-215">Den försöker hello åtgärden tre gånger innan åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="424d7-215">It retries hello operation three times before failing.</span></span>

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

<span data-ttu-id="424d7-216">hello följande exempel visar hur toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="424d7-216">hello following examples demonstrate how toouse this function.</span></span>

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <span data-ttu-id="424d7-217"><a name="helpermethods"></a>Hjälpmetoder för anpassade skript</span><span class="sxs-lookup"><span data-stu-id="424d7-217"><a name="helpermethods"></a>Helper methods for custom scripts</span></span>

<span data-ttu-id="424d7-218">Skriptet åtgärd helper metoder är verktyg som du kan använda vid skrivning till anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="424d7-218">Script action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="424d7-219">Metoderna finns i den[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) skript.</span><span class="sxs-lookup"><span data-stu-id="424d7-219">These methods are contained in the[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script.</span></span> <span data-ttu-id="424d7-220">Använd följande toodownload hello och använda dem som en del av skriptet:</span><span class="sxs-lookup"><span data-stu-id="424d7-220">Use hello following toodownload and use them as part of your script:</span></span>

```bash
# Import hello helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

<span data-ttu-id="424d7-221">hello efter hjälpprogram som är tillgängliga för användning i skriptet:</span><span class="sxs-lookup"><span data-stu-id="424d7-221">hello following helpers available for use in your script:</span></span>

| <span data-ttu-id="424d7-222">Helper användning</span><span class="sxs-lookup"><span data-stu-id="424d7-222">Helper usage</span></span> | <span data-ttu-id="424d7-223">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="424d7-223">Description</span></span> |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |<span data-ttu-id="424d7-224">Laddar ned en fil från hello URI toohello angivna sökvägen till källfilen.</span><span class="sxs-lookup"><span data-stu-id="424d7-224">Downloads a file from hello source URI toohello specified file path.</span></span> <span data-ttu-id="424d7-225">Som standard den inte över en befintlig fil.</span><span class="sxs-lookup"><span data-stu-id="424d7-225">By default, it does not overwrite an existing file.</span></span> |
| `untar_file TARFILE DESTDIR` |<span data-ttu-id="424d7-226">Extraherar tar-filen (med `-xf`) toohello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="424d7-226">Extracts a tar file (using `-xf`) toohello destination directory.</span></span> |
| `test_is_headnode` |<span data-ttu-id="424d7-227">Om kördes på en klustrets huvudnod, returnerar 1. annars 0.</span><span class="sxs-lookup"><span data-stu-id="424d7-227">If ran on a cluster head node, return 1; otherwise, 0.</span></span> |
| `test_is_datanode` |<span data-ttu-id="424d7-228">Om hello aktuella noden är en datanod (worker), returnera 1; annars 0.</span><span class="sxs-lookup"><span data-stu-id="424d7-228">If hello current node is a data (worker) node, return a 1; otherwise, 0.</span></span> |
| `test_is_first_datanode` |<span data-ttu-id="424d7-229">Om hello aktuella noden hello första data (worker) nod (namngivna workernode0) returnera 1; annars 0.</span><span class="sxs-lookup"><span data-stu-id="424d7-229">If hello current node is hello first data (worker) node (named workernode0) return a 1; otherwise, 0.</span></span> |
| `get_headnodes` |<span data-ttu-id="424d7-230">Returnera hello fullständigt kvalificerade domännamnet för hello headnodes i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-230">Return hello fully qualified domain name of hello headnodes in hello cluster.</span></span> <span data-ttu-id="424d7-231">Namn är kommaavgränsade.</span><span class="sxs-lookup"><span data-stu-id="424d7-231">Names are comma delimited.</span></span> <span data-ttu-id="424d7-232">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="424d7-232">An empty string is returned on error.</span></span> |
| `get_primary_headnode` |<span data-ttu-id="424d7-233">Hämtar hello fullständigt kvalificerade domännamnet för hello primära headnode.</span><span class="sxs-lookup"><span data-stu-id="424d7-233">Gets hello fully qualified domain name of hello primary headnode.</span></span> <span data-ttu-id="424d7-234">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="424d7-234">An empty string is returned on error.</span></span> |
| `get_secondary_headnode` |<span data-ttu-id="424d7-235">Hämtar hello fullständigt kvalificerade domännamnet för hello sekundära headnode.</span><span class="sxs-lookup"><span data-stu-id="424d7-235">Gets hello fully qualified domain name of hello secondary headnode.</span></span> <span data-ttu-id="424d7-236">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="424d7-236">An empty string is returned on error.</span></span> |
| `get_primary_headnode_number` |<span data-ttu-id="424d7-237">Hämtar hello numeriskt suffix hello primära headnode.</span><span class="sxs-lookup"><span data-stu-id="424d7-237">Gets hello numeric suffix of hello primary headnode.</span></span> <span data-ttu-id="424d7-238">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="424d7-238">An empty string is returned on error.</span></span> |
| `get_secondary_headnode_number` |<span data-ttu-id="424d7-239">Hämtar hello numeriskt suffix hello sekundära headnode.</span><span class="sxs-lookup"><span data-stu-id="424d7-239">Gets hello numeric suffix of hello secondary headnode.</span></span> <span data-ttu-id="424d7-240">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="424d7-240">An empty string is returned on error.</span></span> |

## <span data-ttu-id="424d7-241"><a name="commonusage"></a>Vanliga användningsmönster</span><span class="sxs-lookup"><span data-stu-id="424d7-241"><a name="commonusage"></a>Common usage patterns</span></span>

<span data-ttu-id="424d7-242">Det här avsnittet ger vägledning om att implementera några hello vanliga användningsmönster som som kan uppstå vid skrivning till ett eget skript.</span><span class="sxs-lookup"><span data-stu-id="424d7-242">This section provides guidance on implementing some of hello common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="passing-parameters-tooa-script"></a><span data-ttu-id="424d7-243">Skicka parametrar tooa skript</span><span class="sxs-lookup"><span data-stu-id="424d7-243">Passing parameters tooa script</span></span>

<span data-ttu-id="424d7-244">I vissa fall, kan skriptet kräver parametrar.</span><span class="sxs-lookup"><span data-stu-id="424d7-244">In some cases, your script may require parameters.</span></span> <span data-ttu-id="424d7-245">Du kan exempelvis behöva hello administratörslösenord för hello klustret när du använder hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="424d7-245">For example, you may need hello admin password for hello cluster when using hello Ambari REST API.</span></span>

<span data-ttu-id="424d7-246">Parametrar som skickas toohello skript kallas *positionsparametrarna*, och tilldelas för`$1` för hello första parametern, `$2` för hello andra och så på.</span><span class="sxs-lookup"><span data-stu-id="424d7-246">Parameters passed toohello script are known as *positional parameters*, and are assigned too`$1` for hello first parameter, `$2` for hello second, and so-on.</span></span> <span data-ttu-id="424d7-247">`$0`innehåller hello namnet på själva hello-skriptet.</span><span class="sxs-lookup"><span data-stu-id="424d7-247">`$0` contains hello name of hello script itself.</span></span>

<span data-ttu-id="424d7-248">Argumenten toohello skript som parametrar ska omges av enkla citattecken (').</span><span class="sxs-lookup"><span data-stu-id="424d7-248">Values passed toohello script as parameters should be enclosed by single quotes (').</span></span> <span data-ttu-id="424d7-249">På så sätt att hello skickades värde behandlas som en literal.</span><span class="sxs-lookup"><span data-stu-id="424d7-249">Doing so ensures that hello passed value is treated as a literal.</span></span>

### <a name="setting-environment-variables"></a><span data-ttu-id="424d7-250">Ställa in miljövariabler</span><span class="sxs-lookup"><span data-stu-id="424d7-250">Setting environment variables</span></span>

<span data-ttu-id="424d7-251">Ange en miljövariabel utförs av hello följande instruktion:</span><span class="sxs-lookup"><span data-stu-id="424d7-251">Setting an environment variable is performed by hello following statement:</span></span>

    VARIABLENAME=value

<span data-ttu-id="424d7-252">Där är VARIABELNAMN hello namnet på hello variabel.</span><span class="sxs-lookup"><span data-stu-id="424d7-252">Where VARIABLENAME is hello name of hello variable.</span></span> <span data-ttu-id="424d7-253">tooaccess hello variabel, Använd `$VARIABLENAME`.</span><span class="sxs-lookup"><span data-stu-id="424d7-253">tooaccess hello variable, use `$VARIABLENAME`.</span></span> <span data-ttu-id="424d7-254">Till exempel tooassign ett värde som tillhandahålls av namngivna parametrar som en miljövariabel med namnet lösenord, använder du följande instruktion hello:</span><span class="sxs-lookup"><span data-stu-id="424d7-254">For example, tooassign a value provided by a positional parameter as an environment variable named PASSWORD, you would use hello following statement:</span></span>

    PASSWORD=$1

<span data-ttu-id="424d7-255">Efterföljande åtkomst toohello information kan sedan använda `$PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="424d7-255">Subsequent access toohello information could then use `$PASSWORD`.</span></span>

<span data-ttu-id="424d7-256">Miljövariabler som anges i hello skript finns bara i hello omfattning hello skript.</span><span class="sxs-lookup"><span data-stu-id="424d7-256">Environment variables set within hello script only exist within hello scope of hello script.</span></span> <span data-ttu-id="424d7-257">I vissa fall kan behöva du tooadd systemomfattande miljövariabler som finns kvar efter hello skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="424d7-257">In some cases, you may need tooadd system-wide environment variables that will persist after hello script has finished.</span></span> <span data-ttu-id="424d7-258">tooadd systemomfattande miljövariabler, Lägg till hello variabel för`/etc/environment`.</span><span class="sxs-lookup"><span data-stu-id="424d7-258">tooadd system-wide environment variables, add hello variable too`/etc/environment`.</span></span> <span data-ttu-id="424d7-259">Till exempel hello följande uttryck lägger till `HADOOP_CONF_DIR`:</span><span class="sxs-lookup"><span data-stu-id="424d7-259">For example, hello following statement adds `HADOOP_CONF_DIR`:</span></span>

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a><span data-ttu-id="424d7-260">Åtkomst till toolocations där hello anpassade skript lagras</span><span class="sxs-lookup"><span data-stu-id="424d7-260">Access toolocations where hello custom scripts are stored</span></span>

<span data-ttu-id="424d7-261">Skript som används toocustomize ett kluster måste toobe lagras i något av följande platser hello:</span><span class="sxs-lookup"><span data-stu-id="424d7-261">Scripts used toocustomize a cluster needs toobe stored in one of hello following locations:</span></span>

* <span data-ttu-id="424d7-262">En __Azure Storage-konto__ som är associerad med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-262">An __Azure Storage account__ that is associated with hello cluster.</span></span>

* <span data-ttu-id="424d7-263">En __ytterligare lagringskonto__ som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-263">An __additional storage account__ associated with hello cluster.</span></span>

* <span data-ttu-id="424d7-264">En __offentligt läsbar URI__.</span><span class="sxs-lookup"><span data-stu-id="424d7-264">A __publicly readable URI__.</span></span> <span data-ttu-id="424d7-265">Till exempel en URL toodata lagras på OneDrive, Dropbox eller andra filer som är värd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="424d7-265">For example, a URL toodata stored on OneDrive, Dropbox, or other file hosting service.</span></span>

* <span data-ttu-id="424d7-266">En __Azure Data Lake Store-konto__ som är associerad med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-266">An __Azure Data Lake Store account__ that is associated with hello HDInsight cluster.</span></span> <span data-ttu-id="424d7-267">Mer information om hur du använder Azure Data Lake Store med HDInsight finns [skapar ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="424d7-267">For more information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="424d7-268">hello service principal HDInsight använder tooaccess Data Lake Store måste ha läsbehörighet toohello skript.</span><span class="sxs-lookup"><span data-stu-id="424d7-268">hello service principal HDInsight uses tooaccess Data Lake Store must have read access toohello script.</span></span>

<span data-ttu-id="424d7-269">Resurser som används av hello skriptet måste också vara offentligt tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="424d7-269">Resources used by hello script must also be publicly available.</span></span>

<span data-ttu-id="424d7-270">Lagra hello filer i en Azure Storage-konto eller ett Azure Data Lake Store ger snabb åtkomst som både i hello Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="424d7-270">Storing hello files in an Azure Storage account or Azure Data Lake Store provides fast access, as both within hello Azure network.</span></span>

> [!NOTE]
> <span data-ttu-id="424d7-271">hello URI-format som används för tooreference hello skript skiljer sig beroende på hello tjänsten används.</span><span class="sxs-lookup"><span data-stu-id="424d7-271">hello URI format used tooreference hello script differs depending on hello service being used.</span></span> <span data-ttu-id="424d7-272">Storage-konton som är associerade med hello HDInsight-kluster, använda `wasb://` eller `wasbs://`.</span><span class="sxs-lookup"><span data-stu-id="424d7-272">For storage accounts associated with hello HDInsight cluster, use `wasb://` or `wasbs://`.</span></span> <span data-ttu-id="424d7-273">Offentligt läsbar URI: er använda `http://` eller `https://`.</span><span class="sxs-lookup"><span data-stu-id="424d7-273">For publicly readable URIs, use `http://` or `https://`.</span></span> <span data-ttu-id="424d7-274">Data Lake Store använder `adl://`.</span><span class="sxs-lookup"><span data-stu-id="424d7-274">For Data Lake Store, use `adl://`.</span></span>

### <a name="checking-hello-operating-system-version"></a><span data-ttu-id="424d7-275">Kontrollerar hello operativsystemets version</span><span class="sxs-lookup"><span data-stu-id="424d7-275">Checking hello operating system version</span></span>

<span data-ttu-id="424d7-276">Olika versioner av HDInsight är beroende av specifika versioner av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="424d7-276">Different versions of HDInsight rely on specific versions of Ubuntu.</span></span> <span data-ttu-id="424d7-277">Det kan finnas skillnader mellan OS-versioner måste du kontrollera i skriptet.</span><span class="sxs-lookup"><span data-stu-id="424d7-277">There may be differences between OS versions that you must check for in your script.</span></span> <span data-ttu-id="424d7-278">Du kan till exempel måste tooinstall en binärfil som är bundet toohello version av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="424d7-278">For example, you may need tooinstall a binary that is tied toohello version of Ubuntu.</span></span>

<span data-ttu-id="424d7-279">toocheck hello OS-version, Använd `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="424d7-279">toocheck hello OS version, use `lsb_release`.</span></span> <span data-ttu-id="424d7-280">Hello följande skript visar exempelvis hur tooreference specifika tar filen beroende på hello OS-version:</span><span class="sxs-lookup"><span data-stu-id="424d7-280">For example, hello following script demonstrates how tooreference a specific tar file depending on hello OS version:</span></span>

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <span data-ttu-id="424d7-281"><a name="deployScript"></a>Checklista för distribution av en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="424d7-281"><a name="deployScript"></a>Checklist for deploying a script action</span></span>

<span data-ttu-id="424d7-282">Här är hello steg som vi har tagit när du förbereder toodeploy dessa skript:</span><span class="sxs-lookup"><span data-stu-id="424d7-282">Here are hello steps we took when preparing toodeploy these scripts:</span></span>

* <span data-ttu-id="424d7-283">Placera hello-filer som innehåller hello anpassade skript på en plats som kan nås av hello klusternoder under distributionen.</span><span class="sxs-lookup"><span data-stu-id="424d7-283">Put hello files that contain hello custom scripts in a place that is accessible by hello cluster nodes during deployment.</span></span> <span data-ttu-id="424d7-284">Till exempel hello standardlagring för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-284">For example, hello default storage for hello cluster.</span></span> <span data-ttu-id="424d7-285">Filer kan också lagras i offentligt läsbar värdtjänster.</span><span class="sxs-lookup"><span data-stu-id="424d7-285">Files can also be stored in publicly readable hosting services.</span></span>
* <span data-ttu-id="424d7-286">Kontrollera att skriptet hello är impotent.</span><span class="sxs-lookup"><span data-stu-id="424d7-286">Verify that hello script is impotent.</span></span> <span data-ttu-id="424d7-287">På så sätt kan hello skriptet toobe köras flera gånger på hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="424d7-287">Doing so allows hello script toobe executed multiple times on hello same node.</span></span>
* <span data-ttu-id="424d7-288">Använd en tillfällig katalog /tmp tookeep hello nedladdade filer som används av hello skript och sedan rensa dem efter skript har körts.</span><span class="sxs-lookup"><span data-stu-id="424d7-288">Use a temporary file directory /tmp tookeep hello downloaded files used by hello scripts and then clean them up after scripts have executed.</span></span>
* <span data-ttu-id="424d7-289">Om inställningar för OS-nivå eller Hadoop service configuration-filer har ändrats kanske toorestart HDInsight tjänster.</span><span class="sxs-lookup"><span data-stu-id="424d7-289">If OS-level settings or Hadoop service configuration files are changed, you may want toorestart HDInsight services.</span></span>

## <span data-ttu-id="424d7-290"><a name="runScriptAction"></a>Hur toorun en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="424d7-290"><a name="runScriptAction"></a>How toorun a script action</span></span>

<span data-ttu-id="424d7-291">Du kan använda skriptet åtgärder toocustomize HDInsight-kluster med hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="424d7-291">You can use script actions toocustomize HDInsight clusters using hello following methods:</span></span>

* <span data-ttu-id="424d7-292">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="424d7-292">Azure portal</span></span>
* <span data-ttu-id="424d7-293">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="424d7-293">Azure PowerShell</span></span>
* <span data-ttu-id="424d7-294">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="424d7-294">Azure Resource Manager templates</span></span>
* <span data-ttu-id="424d7-295">Hej HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="424d7-295">hello HDInsight .NET SDK.</span></span>

<span data-ttu-id="424d7-296">Mer information om hur du använder varje metod finns [hur toouse skript åtgärden](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="424d7-296">For more information on using each method, see [How toouse script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="424d7-297"><a name="sampleScripts"></a>Anpassade skriptexempel</span><span class="sxs-lookup"><span data-stu-id="424d7-297"><a name="sampleScripts"></a>Custom script samples</span></span>

<span data-ttu-id="424d7-298">Microsoft tillhandahåller exempelskript tooinstall komponenter på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-298">Microsoft provides sample scripts tooinstall components on an HDInsight cluster.</span></span> <span data-ttu-id="424d7-299">Se följande länkar för flera exempel skriptåtgärder hello.</span><span class="sxs-lookup"><span data-stu-id="424d7-299">See hello following links for more example script actions.</span></span>

* [<span data-ttu-id="424d7-300">Installera och använda Hue på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="424d7-300">Install and use Hue on HDInsight clusters</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="424d7-301">Installera och använda Solr på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="424d7-301">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="424d7-302">Installera och använda Giraph på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="424d7-302">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="424d7-303">Installera eller uppgradera Mono på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="424d7-303">Install or upgrade Mono on HDInsight clusters</span></span>](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a><span data-ttu-id="424d7-304">Felsökning</span><span class="sxs-lookup"><span data-stu-id="424d7-304">Troubleshooting</span></span>

<span data-ttu-id="424d7-305">hello följande är fel som kan uppstå när du använder skript som du har utvecklat:</span><span class="sxs-lookup"><span data-stu-id="424d7-305">hello following are errors you may encounter when using scripts you have developed:</span></span>

<span data-ttu-id="424d7-306">**Fel**: `$'\r': command not found`.</span><span class="sxs-lookup"><span data-stu-id="424d7-306">**Error**: `$'\r': command not found`.</span></span> <span data-ttu-id="424d7-307">Ibland följt av `syntax error: unexpected end of file`.</span><span class="sxs-lookup"><span data-stu-id="424d7-307">Sometimes followed by `syntax error: unexpected end of file`.</span></span>

<span data-ttu-id="424d7-308">*Orsak*: det här felet uppstår om hello rader i ett skript som avslutas med CRLF.</span><span class="sxs-lookup"><span data-stu-id="424d7-308">*Cause*: This error is caused when hello lines in a script end with CRLF.</span></span> <span data-ttu-id="424d7-309">UNIX-system förväntar sig endast LF som hello rad avslutas.</span><span class="sxs-lookup"><span data-stu-id="424d7-309">Unix systems expect only LF as hello line ending.</span></span>

<span data-ttu-id="424d7-310">Det här problemet inträffar oftast när hello skript har skapats på en Windows-miljö CRLF är en gemensam rader som slutar med för många textredigerare på Windows.</span><span class="sxs-lookup"><span data-stu-id="424d7-310">This problem most often occurs when hello script is authored on a Windows environment, as CRLF is a common line ending for many text editors on Windows.</span></span>

<span data-ttu-id="424d7-311">*Lösning*: om det är ett alternativ i en textredigerare, Välj Unix-format eller LF för hello rad avslutas.</span><span class="sxs-lookup"><span data-stu-id="424d7-311">*Resolution*: If it is an option in your text editor, select Unix format or LF for hello line ending.</span></span> <span data-ttu-id="424d7-312">Du kan även använda följande kommandon på en Unix system toochange hello CRLF tooan LF hello:</span><span class="sxs-lookup"><span data-stu-id="424d7-312">You may also use hello following commands on a Unix system toochange hello CRLF tooan LF:</span></span>

> [!NOTE]
> <span data-ttu-id="424d7-313">hello är följande kommandon ungefär att de ska ändra hello CRLF rad ändelser tooLF.</span><span class="sxs-lookup"><span data-stu-id="424d7-313">hello following commands are roughly equivalent in that they should change hello CRLF line endings tooLF.</span></span> <span data-ttu-id="424d7-314">Välj en baserat på hello-verktyg som finns på datorn.</span><span class="sxs-lookup"><span data-stu-id="424d7-314">Select one based on hello utilities available on your system.</span></span>

| <span data-ttu-id="424d7-315">Kommando</span><span class="sxs-lookup"><span data-stu-id="424d7-315">Command</span></span> | <span data-ttu-id="424d7-316">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="424d7-316">Notes</span></span> |
| --- | --- |
| `unix2dos -b INFILE` |<span data-ttu-id="424d7-317">hello originalfilen säkerhetskopieras med en. BAK-tillägg</span><span class="sxs-lookup"><span data-stu-id="424d7-317">hello original file is backed up with a .BAK extension</span></span> |
| `tr -d '\r' < INFILE > OUTFILE` |<span data-ttu-id="424d7-318">UTFIL innehåller en version med endast LF ändelser</span><span class="sxs-lookup"><span data-stu-id="424d7-318">OUTFILE contains a version with only LF endings</span></span> |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | <span data-ttu-id="424d7-319">Ändrar hello-filen direkt</span><span class="sxs-lookup"><span data-stu-id="424d7-319">Modifies hello file directly</span></span> |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |<span data-ttu-id="424d7-320">UTFIL innehåller en version med endast LF ändelser.</span><span class="sxs-lookup"><span data-stu-id="424d7-320">OUTFILE contains a version with only LF endings.</span></span> |

<span data-ttu-id="424d7-321">**Fel**: `line 1: #!/usr/bin/env: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="424d7-321">**Error**: `line 1: #!/usr/bin/env: No such file or directory`.</span></span>

<span data-ttu-id="424d7-322">*Orsak*: det här felet uppstår när hello skript har sparats som UTF-8 en Byte ordning markering (BOM).</span><span class="sxs-lookup"><span data-stu-id="424d7-322">*Cause*: This error occurs when hello script was saved as UTF-8 with a Byte Order Mark (BOM).</span></span>

<span data-ttu-id="424d7-323">*Lösning*: spara hello-fil som ASCII eller som UTF-8 utan en struktur.</span><span class="sxs-lookup"><span data-stu-id="424d7-323">*Resolution*: Save hello file either as ASCII, or as UTF-8 without a BOM.</span></span> <span data-ttu-id="424d7-324">Du kan även använda följande kommando på en Linux eller Unix system-toocreate en fil utan hello BOM hello:</span><span class="sxs-lookup"><span data-stu-id="424d7-324">You may also use hello following command on a Linux or Unix system toocreate a file without hello BOM:</span></span>

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

<span data-ttu-id="424d7-325">Ersätt `INFILE` med hello-fil som innehåller hello BOM.</span><span class="sxs-lookup"><span data-stu-id="424d7-325">Replace `INFILE` with hello file containing hello BOM.</span></span> <span data-ttu-id="424d7-326">`OUTFILE`måste vara ett nytt filnamn som innehåller hello skript utan hello BOM.</span><span class="sxs-lookup"><span data-stu-id="424d7-326">`OUTFILE` should be a new file name, which contains hello script without hello BOM.</span></span>

## <span data-ttu-id="424d7-327"><a name="seeAlso"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="424d7-327"><a name="seeAlso"></a>Next steps</span></span>

* <span data-ttu-id="424d7-328">Lär dig hur för[anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="424d7-328">Learn how too[Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
* <span data-ttu-id="424d7-329">Använd hello [HDInsight .NET SDK referens](https://msdn.microsoft.com/library/mt271028.aspx) toolearn mer om hur du skapar .NET-program som hanterar HDInsight</span><span class="sxs-lookup"><span data-stu-id="424d7-329">Use hello [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx) toolearn more about creating .NET applications that manage HDInsight</span></span>
* <span data-ttu-id="424d7-330">Använd hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn hur toouse REST tooperform hanteringsåtgärder på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="424d7-330">Use hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn how toouse REST tooperform management actions on HDInsight clusters.</span></span>
