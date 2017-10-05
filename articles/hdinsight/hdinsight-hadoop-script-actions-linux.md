---
title: "Utveckling av skriptåtgärder med Linux-baserat HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du använder Bash-skript för att anpassa Linux-baserade HDInsight-kluster. Funktionen skript åtgärd i HDInsight kan du köra skript under eller efter att klustret har skapats. Kan använda skript för att ändra inställningar för klustrets eller installera ytterligare programvara."
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
ms.openlocfilehash: 7f1a0bd8c7e60770d376f10eaea136a55c632c5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="script-action-development-with-hdinsight"></a><span data-ttu-id="1cac6-105">Skriptutveckling med HDInsight</span><span class="sxs-lookup"><span data-stu-id="1cac6-105">Script action development with HDInsight</span></span>

<span data-ttu-id="1cac6-106">Lär dig hur du anpassar ditt HDInsight-kluster med hjälp av Bash-skript.</span><span class="sxs-lookup"><span data-stu-id="1cac6-106">Learn how to customize your HDInsight cluster using Bash scripts.</span></span> <span data-ttu-id="1cac6-107">Skriptåtgärder är ett sätt att anpassa HDInsight under eller efter att klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="1cac6-107">Script actions are a way to customize HDInsight during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1cac6-108">Stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="1cac6-108">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="1cac6-109">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="1cac6-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1cac6-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1cac6-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="what-are-script-actions"></a><span data-ttu-id="1cac6-111">Vad är skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="1cac6-111">What are script actions</span></span>

<span data-ttu-id="1cac6-112">Skriptåtgärder är Bash-skript som Azure körs på klusternoderna för att göra konfigurationsändringar eller installera programvara.</span><span class="sxs-lookup"><span data-stu-id="1cac6-112">Script actions are Bash scripts that Azure runs on the cluster nodes to make configuration changes or install software.</span></span> <span data-ttu-id="1cac6-113">En skriptåtgärd körs som rot och ger fullständig behörighet till klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="1cac6-113">A script action is executed as root, and provides full access rights to the cluster nodes.</span></span>

<span data-ttu-id="1cac6-114">Skriptåtgärder kan tillämpas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1cac6-114">Script actions can be applied through the following methods:</span></span>

| <span data-ttu-id="1cac6-115">Använd den här metoden för att använda ett skript...</span><span class="sxs-lookup"><span data-stu-id="1cac6-115">Use this method to apply a script...</span></span> | <span data-ttu-id="1cac6-116">Skapa kluster under...</span><span class="sxs-lookup"><span data-stu-id="1cac6-116">During cluster creation...</span></span> | <span data-ttu-id="1cac6-117">På ett kluster som körs...</span><span class="sxs-lookup"><span data-stu-id="1cac6-117">On a running cluster...</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="1cac6-118">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1cac6-118">Azure portal</span></span> |<span data-ttu-id="1cac6-119">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-119">✓</span></span> |<span data-ttu-id="1cac6-120">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-120">✓</span></span> |
| <span data-ttu-id="1cac6-121">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1cac6-121">Azure PowerShell</span></span> |<span data-ttu-id="1cac6-122">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-122">✓</span></span> |<span data-ttu-id="1cac6-123">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-123">✓</span></span> |
| <span data-ttu-id="1cac6-124">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1cac6-124">Azure CLI</span></span> |&nbsp; |<span data-ttu-id="1cac6-125">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-125">✓</span></span> |
| <span data-ttu-id="1cac6-126">HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1cac6-126">HDInsight .NET SDK</span></span> |<span data-ttu-id="1cac6-127">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-127">✓</span></span> |<span data-ttu-id="1cac6-128">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-128">✓</span></span> |
| <span data-ttu-id="1cac6-129">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="1cac6-129">Azure Resource Manager Template</span></span> |<span data-ttu-id="1cac6-130">✓</span><span class="sxs-lookup"><span data-stu-id="1cac6-130">✓</span></span> |&nbsp; |

<span data-ttu-id="1cac6-131">Mer information om hur du använder dessa metoder för att tillämpa skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1cac6-131">For more information on using these methods to apply script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="1cac6-132"><a name="bestPracticeScripting"></a>Metodtips för skriptutveckling av</span><span class="sxs-lookup"><span data-stu-id="1cac6-132"><a name="bestPracticeScripting"></a>Best practices for script development</span></span>

<span data-ttu-id="1cac6-133">Finns flera bästa praxis att tänka på när du utvecklar ett anpassat skript för ett HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="1cac6-133">When you develop a custom script for an HDInsight cluster, there are several best practices to keep in mind:</span></span>

* [<span data-ttu-id="1cac6-134">Använder Hadoop-version</span><span class="sxs-lookup"><span data-stu-id="1cac6-134">Target the Hadoop version</span></span>](#bPS1)
* [<span data-ttu-id="1cac6-135">Mål OS-Version</span><span class="sxs-lookup"><span data-stu-id="1cac6-135">Target the OS Version</span></span>](#bps10)
* [<span data-ttu-id="1cac6-136">Ange stabil länkar till skriptresurser</span><span class="sxs-lookup"><span data-stu-id="1cac6-136">Provide stable links to script resources</span></span>](#bPS2)
* [<span data-ttu-id="1cac6-137">Använda fördefinierade kompilerade resurser</span><span class="sxs-lookup"><span data-stu-id="1cac6-137">Use pre-compiled resources</span></span>](#bPS4)
* [<span data-ttu-id="1cac6-138">Se till att klustret anpassning skriptet idempotent</span><span class="sxs-lookup"><span data-stu-id="1cac6-138">Ensure that the cluster customization script is idempotent</span></span>](#bPS3)
* [<span data-ttu-id="1cac6-139">Garantera hög tillgänglighet för kluster-arkitektur</span><span class="sxs-lookup"><span data-stu-id="1cac6-139">Ensure high availability of the cluster architecture</span></span>](#bPS5)
* [<span data-ttu-id="1cac6-140">Konfigurera anpassade komponenter om du vill använda Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="1cac6-140">Configure the custom components to use Azure Blob storage</span></span>](#bPS6)
* [<span data-ttu-id="1cac6-141">Skriva information till STDOUT och STDERR</span><span class="sxs-lookup"><span data-stu-id="1cac6-141">Write information to STDOUT and STDERR</span></span>](#bPS7)
* [<span data-ttu-id="1cac6-142">Spara filer i ASCII-format med LF radbrytningar</span><span class="sxs-lookup"><span data-stu-id="1cac6-142">Save files as ASCII with LF line endings</span></span>](#bps8)
* [<span data-ttu-id="1cac6-143">Använda logik för att återställa från tillfälliga fel</span><span class="sxs-lookup"><span data-stu-id="1cac6-143">Use retry logic to recover from transient errors</span></span>](#bps9)

> [!IMPORTANT]
> <span data-ttu-id="1cac6-144">Skriptåtgärder måste slutföras inom 60 minuter eller processen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1cac6-144">Script actions must complete within 60 minutes or the process fails.</span></span> <span data-ttu-id="1cac6-145">Under noden etableringen körs skriptet samtidigt med andra processer för installation och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1cac6-145">During node provisioning, the script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="1cac6-146">Konkurrens om resurser, till exempel CPU-tid eller nätverket bandbredd kan göra att skriptet tar längre tid att slutföra än i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1cac6-146">Competition for resources such as CPU time or network bandwidth may cause the script to take longer to finish than it does in your development environment.</span></span>

### <span data-ttu-id="1cac6-147"><a name="bPS1"></a>Använder Hadoop-version</span><span class="sxs-lookup"><span data-stu-id="1cac6-147"><a name="bPS1"></a>Target the Hadoop version</span></span>

<span data-ttu-id="1cac6-148">Olika versioner av HDInsight har olika versioner av Hadoop-tjänster och komponenter installeras.</span><span class="sxs-lookup"><span data-stu-id="1cac6-148">Different versions of HDInsight have different versions of Hadoop services and components installed.</span></span> <span data-ttu-id="1cac6-149">Om skriptet förväntar en viss version av en tjänst eller en komponent, bör du bara använda skriptet med versionen av HDInsight som innehåller de nödvändiga komponenterna.</span><span class="sxs-lookup"><span data-stu-id="1cac6-149">If your script expects a specific version of a service or component, you should only use the script with the version of HDInsight that includes the required components.</span></span> <span data-ttu-id="1cac6-150">Du kan hitta information om komponenten-versioner som ingår i HDInsight med hjälp av den [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-150">You can find information on component versions included with HDInsight using the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

### <span data-ttu-id="1cac6-151"><a name="bps10"></a>Målversionen OS</span><span class="sxs-lookup"><span data-stu-id="1cac6-151"><a name="bps10"></a> Target the OS version</span></span>

<span data-ttu-id="1cac6-152">Linux-baserat HDInsight baseras på Ubuntu Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="1cac6-152">Linux-based HDInsight is based on the Ubuntu Linux distribution.</span></span> <span data-ttu-id="1cac6-153">Olika versioner av HDInsight förlitar sig på olika versioner av Ubuntu som kan ändra hur skriptet fungerar.</span><span class="sxs-lookup"><span data-stu-id="1cac6-153">Different versions of HDInsight rely on different versions of Ubuntu, which may change how your script behaves.</span></span> <span data-ttu-id="1cac6-154">Till exempel HDInsight 3,4 och tidigare baseras på Ubuntu-versioner som använder Upstart.</span><span class="sxs-lookup"><span data-stu-id="1cac6-154">For example, HDInsight 3.4 and earlier are based on Ubuntu versions that use Upstart.</span></span> <span data-ttu-id="1cac6-155">Version 3.5 är baserad på Ubuntu 16.04 som använder Systemd.</span><span class="sxs-lookup"><span data-stu-id="1cac6-155">Version 3.5 is based on Ubuntu 16.04, which uses Systemd.</span></span> <span data-ttu-id="1cac6-156">Systemd och Upstart förlitar sig på olika kommandon så att skriptet ska skrivas till fungerar med båda.</span><span class="sxs-lookup"><span data-stu-id="1cac6-156">Systemd and Upstart rely on different commands, so your script should be written to work with both.</span></span>

<span data-ttu-id="1cac6-157">En annan viktig skillnad mellan HDInsight 3.4 och 3.5 är att `JAVA_HOME` nu pekar på Java 8.</span><span class="sxs-lookup"><span data-stu-id="1cac6-157">Another important difference between HDInsight 3.4 and 3.5 is that `JAVA_HOME` now points to Java 8.</span></span>

<span data-ttu-id="1cac6-158">Du kan kontrollera versionen av Operativsystemet med hjälp av `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-158">You can check the OS version by using `lsb_release`.</span></span> <span data-ttu-id="1cac6-159">Följande kod visar hur du avgör om skriptet körs på Ubuntu 14 eller 16:</span><span class="sxs-lookup"><span data-stu-id="1cac6-159">The following code demonstrates how to determine if the script is running on Ubuntu 14 or 16:</span></span>

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

<span data-ttu-id="1cac6-160">Du hittar den fullständig skript som innehåller dessa kodavsnitt på https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span><span class="sxs-lookup"><span data-stu-id="1cac6-160">You can find the full script that contains these snippets at https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span></span>

<span data-ttu-id="1cac6-161">Versionen av Ubuntu som används av HDInsight finns i [HDInsight komponentversionen](hdinsight-component-versioning.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-161">For the version of Ubuntu that is used by HDInsight, see the [HDInsight component version](hdinsight-component-versioning.md) document.</span></span>

<span data-ttu-id="1cac6-162">Om du vill förstå skillnaderna mellan Systemd och Upstart finns [Systemd för Upstart användare](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span><span class="sxs-lookup"><span data-stu-id="1cac6-162">To understand the differences between Systemd and Upstart, see [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span></span>

### <span data-ttu-id="1cac6-163"><a name="bPS2"></a>Ange stabil länkar till skriptresurser</span><span class="sxs-lookup"><span data-stu-id="1cac6-163"><a name="bPS2"></a>Provide stable links to script resources</span></span>

<span data-ttu-id="1cac6-164">Skript och associerade resurser måste vara tillgängliga under hela livslängden för klustret.</span><span class="sxs-lookup"><span data-stu-id="1cac6-164">The script and associated resources must remain available throughout the lifetime of the cluster.</span></span> <span data-ttu-id="1cac6-165">Dessa resurser krävs om nya noder läggs till i klustret under skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1cac6-165">These resources are required if new nodes are added to the cluster during scaling operations.</span></span>

<span data-ttu-id="1cac6-166">Det bästa sättet är att hämta och arkivera allt innehåll i ett Azure Storage-konto på din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1cac6-166">The best practice is to download and archive everything in an Azure Storage account on your subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1cac6-167">Storage-konto som används måste vara standardkontot för lagring för klustret eller en offentlig, skrivskyddad behållare för andra storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1cac6-167">The storage account used must be the default storage account for the cluster or a public, read-only container on any other storage account.</span></span>

<span data-ttu-id="1cac6-168">Till exempel exemplen som tillhandahålls av Microsoft lagras i den [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1cac6-168">For example, the samples provided by Microsoft are stored in the [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage account.</span></span> <span data-ttu-id="1cac6-169">Detta är en offentlig, skrivskyddad behållare som underhålls av HDInsight-teamet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-169">This is a public, read-only container maintained by the HDInsight team.</span></span>

### <span data-ttu-id="1cac6-170"><a name="bPS4"></a>Använda fördefinierade kompilerade resurser</span><span class="sxs-lookup"><span data-stu-id="1cac6-170"><a name="bPS4"></a>Use pre-compiled resources</span></span>

<span data-ttu-id="1cac6-171">Undvik åtgärder som sammanställer resurser från källkod för att minska den tid det tar för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-171">To reduce the time it takes to run the script, avoid operations that compile resources from source code.</span></span> <span data-ttu-id="1cac6-172">Till exempel före kompilera resurser och lagra dem i en blob med Azure Storage-konto i samma datacenter som HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1cac6-172">For example, pre-compile resources and store them in an Azure Storage account blob in the same data center as HDInsight.</span></span>

### <span data-ttu-id="1cac6-173"><a name="bPS3"></a>Se till att klustret anpassning skriptet idempotent</span><span class="sxs-lookup"><span data-stu-id="1cac6-173"><a name="bPS3"></a>Ensure that the cluster customization script is idempotent</span></span>

<span data-ttu-id="1cac6-174">Skript måste vara idempotent.</span><span class="sxs-lookup"><span data-stu-id="1cac6-174">Scripts must be idempotent.</span></span> <span data-ttu-id="1cac6-175">Om skriptet körs flera gånger, måste den returnera klustret till samma tillstånd varje gång.</span><span class="sxs-lookup"><span data-stu-id="1cac6-175">If the script runs multiple times, it should return the cluster to the same state every time.</span></span>

<span data-ttu-id="1cac6-176">Till exempel ett skript som ändrar konfigurationsfiler bör inte lägga till dubblettposter om körde flera gånger.</span><span class="sxs-lookup"><span data-stu-id="1cac6-176">For example, a script that modifies configuration files should not add duplicate entries if ran multiple times.</span></span>

### <span data-ttu-id="1cac6-177"><a name="bPS5"></a>Garantera hög tillgänglighet för kluster-arkitektur</span><span class="sxs-lookup"><span data-stu-id="1cac6-177"><a name="bPS5"></a>Ensure high availability of the cluster architecture</span></span>

<span data-ttu-id="1cac6-178">Linux-baserade HDInsight-kluster tillhandahåller två huvudnoderna som är aktiva i klustret och skriptåtgärder köras på båda noderna.</span><span class="sxs-lookup"><span data-stu-id="1cac6-178">Linux-based HDInsight clusters provide two head nodes that are active within the cluster, and script actions run on both nodes.</span></span> <span data-ttu-id="1cac6-179">Om de komponenter du installerar förväntas bara en huvudnod kan du inte installera komponenterna på båda huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="1cac6-179">If the components you install expect only one head node, do not install the components on both head nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1cac6-180">Tjänster som tillhandahålls som en del av HDInsight är utformade för att växla över mellan två huvudnoderna efter behov.</span><span class="sxs-lookup"><span data-stu-id="1cac6-180">Services provided as part of HDInsight are designed to fail over between the two head nodes as needed.</span></span> <span data-ttu-id="1cac6-181">Den här funktionen har inte utökats till anpassade komponenter som installeras via skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1cac6-181">This functionality is not extended to custom components installed through script actions.</span></span> <span data-ttu-id="1cac6-182">Om du behöver hög tillgänglighet för anpassade komponenter måste du implementera en egen mekanism för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-182">If you need high availability for custom components, you must implement your own failover mechanism.</span></span>

### <span data-ttu-id="1cac6-183"><a name="bPS6"></a>Konfigurera anpassade komponenter om du vill använda Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="1cac6-183"><a name="bPS6"></a>Configure the custom components to use Azure Blob storage</span></span>

<span data-ttu-id="1cac6-184">Komponenter som installeras på klustret kan ha en standardkonfiguration som använder Hadoop Distributed File System (HDFS) lagring.</span><span class="sxs-lookup"><span data-stu-id="1cac6-184">Components that you install on the cluster might have a default configuration that uses Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="1cac6-185">HDInsight använder antingen Azure Storage eller Azure Data Lake Store som standardlagring.</span><span class="sxs-lookup"><span data-stu-id="1cac6-185">HDInsight uses either Azure Storage or Data Lake Store as the default storage.</span></span> <span data-ttu-id="1cac6-186">Båda ger ett kompatibelt HDFS-filsystem som kvarstår data även om klustret har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="1cac6-186">Both provide an HDFS compatible file system that persists data even if the cluster is deleted.</span></span> <span data-ttu-id="1cac6-187">Du kan behöva konfigurera komponenter som du installerar för att använda WASB eller ADL i stället för HDFS.</span><span class="sxs-lookup"><span data-stu-id="1cac6-187">You may need to configure components you install to use WASB or ADL instead of HDFS.</span></span>

<span data-ttu-id="1cac6-188">Du behöver inte ange filsystemet för de flesta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1cac6-188">For most operations, you do not need to specify the file system.</span></span> <span data-ttu-id="1cac6-189">Till exempel följande giraph examples.jar filen kopieras från det lokala filsystemet till klusterlagringen:</span><span class="sxs-lookup"><span data-stu-id="1cac6-189">For example, the following copies the giraph-examples.jar file from the local file system to cluster storage:</span></span>

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

<span data-ttu-id="1cac6-190">I detta exempel på `hdfs` kommando använder transparent standard klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-190">In this example, the `hdfs` command transparently uses the default cluster storage.</span></span> <span data-ttu-id="1cac6-191">Du kan behöva ange URI för vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1cac6-191">For some operations, you may need to specify the URI.</span></span> <span data-ttu-id="1cac6-192">Till exempel `adl:///example/jars` för Data Lake Store eller `wasb:///example/jars` för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1cac6-192">For example, `adl:///example/jars` for Data Lake Store or `wasb:///example/jars` for Azure Storage.</span></span>

### <span data-ttu-id="1cac6-193"><a name="bPS7"></a>Skriva information till STDOUT och STDERR</span><span class="sxs-lookup"><span data-stu-id="1cac6-193"><a name="bPS7"></a>Write information to STDOUT and STDERR</span></span>

<span data-ttu-id="1cac6-194">HDInsight loggar utdata från skriptet som skrivs till STDOUT och STDERR.</span><span class="sxs-lookup"><span data-stu-id="1cac6-194">HDInsight logs script output that is written to STDOUT and STDERR.</span></span> <span data-ttu-id="1cac6-195">Du kan visa den här informationen med Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-195">You can view this information using the Ambari web UI.</span></span>

> [!NOTE]
> <span data-ttu-id="1cac6-196">Ambari är endast tillgänglig om klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="1cac6-196">Ambari is only available if the cluster is successfully created.</span></span> <span data-ttu-id="1cac6-197">Om du använder en skriptåtgärd under klustret har skapats och skapa misslyckas, finns i avsnittet om felsökning [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) för andra sätt att komma åt loggade informationen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-197">If you use a script action during cluster creation, and creation fails, see the troubleshooting section [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) for other ways of accessing logged information.</span></span>

<span data-ttu-id="1cac6-198">De flesta verktyg och Installationspaketen skriva redan information till STDOUT och STDERR, men du kanske vill lägga till ytterligare loggning.</span><span class="sxs-lookup"><span data-stu-id="1cac6-198">Most utilities and installation packages already write information to STDOUT and STDERR, however you may want to add additional logging.</span></span> <span data-ttu-id="1cac6-199">Om du vill skicka text STDOUT använda `echo`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-199">To send text to STDOUT, use `echo`.</span></span> <span data-ttu-id="1cac6-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1cac6-200">For example:</span></span>

```bash
echo "Getting ready to install Foo"
```

<span data-ttu-id="1cac6-201">Som standard `echo` skickar strängen till STDOUT.</span><span class="sxs-lookup"><span data-stu-id="1cac6-201">By default, `echo` sends the string to STDOUT.</span></span> <span data-ttu-id="1cac6-202">Om du vill styra den till STDERR, lägger du till `>&2` innan `echo`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-202">To direct it to STDERR, add `>&2` before `echo`.</span></span> <span data-ttu-id="1cac6-203">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1cac6-203">For example:</span></span>

```bash
>&2 echo "An error occurred installing Foo"
```

<span data-ttu-id="1cac6-204">Detta omdirigerar information skrivs till STDOUT till STDERR (2) i stället.</span><span class="sxs-lookup"><span data-stu-id="1cac6-204">This redirects information written to STDOUT to STDERR (2) instead.</span></span> <span data-ttu-id="1cac6-205">Mer information om i/o-omdirigering finns [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span><span class="sxs-lookup"><span data-stu-id="1cac6-205">For more information on IO redirection, see [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span></span>

<span data-ttu-id="1cac6-206">Mer information om hur du visar information som loggas av skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span><span class="sxs-lookup"><span data-stu-id="1cac6-206">For more information on viewing information logged by script actions, see [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span></span>

### <span data-ttu-id="1cac6-207"><a name="bps8"></a>Spara filer i ASCII-format med LF radbrytningar</span><span class="sxs-lookup"><span data-stu-id="1cac6-207"><a name="bps8"></a> Save files as ASCII with LF line endings</span></span>

<span data-ttu-id="1cac6-208">Bash-skript ska lagras som ASCII-format med rader som avslutas av LF.</span><span class="sxs-lookup"><span data-stu-id="1cac6-208">Bash scripts should be stored as ASCII format, with lines terminated by LF.</span></span> <span data-ttu-id="1cac6-209">Filer som lagras som UTF-8 eller använda CRLF som rad avslutas kan misslyckas med följande fel:</span><span class="sxs-lookup"><span data-stu-id="1cac6-209">Files that are stored as UTF-8, or use CRLF as the line ending may fail with the following error:</span></span>

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <span data-ttu-id="1cac6-210"><a name="bps9"></a>Använda logik för att återställa från tillfälliga fel</span><span class="sxs-lookup"><span data-stu-id="1cac6-210"><a name="bps9"></a> Use retry logic to recover from transient errors</span></span>

<span data-ttu-id="1cac6-211">När du laddar ned filer som installerar paket med hjälp av lgh get eller andra åtgärder som överför data via internet, misslyckas åtgärden på grund av tillfälliga nätverksfel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-211">When downloading files, installing packages using apt-get, or other actions that transmit data over the internet, the action may fail due to transient networking errors.</span></span> <span data-ttu-id="1cac6-212">Till exempel kanske du kommunicerar med fjärresursen håller på att inte körs på en nod för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1cac6-212">For example, the remote resource you are communicating with may be in the process of failing over to a backup node.</span></span>

<span data-ttu-id="1cac6-213">Du kan implementera logik för att göra ditt skript flexibel att tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-213">To make your script resilient to transient errors, you can implement retry logic.</span></span> <span data-ttu-id="1cac6-214">Följande funktion visar hur du implementerar logik.</span><span class="sxs-lookup"><span data-stu-id="1cac6-214">The following function demonstrates how to implement retry logic.</span></span> <span data-ttu-id="1cac6-215">Den försöker igen tre gånger innan åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1cac6-215">It retries the operation three times before failing.</span></span>

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

<span data-ttu-id="1cac6-216">Följande exempel visar hur du använder den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-216">The following examples demonstrate how to use this function.</span></span>

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <span data-ttu-id="1cac6-217"><a name="helpermethods"></a>Hjälpmetoder för anpassade skript</span><span class="sxs-lookup"><span data-stu-id="1cac6-217"><a name="helpermethods"></a>Helper methods for custom scripts</span></span>

<span data-ttu-id="1cac6-218">Skriptet åtgärd helper metoder är verktyg som du kan använda vid skrivning till anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="1cac6-218">Script action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="1cac6-219">Metoderna finns i den[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) skript.</span><span class="sxs-lookup"><span data-stu-id="1cac6-219">These methods are contained in the[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script.</span></span> <span data-ttu-id="1cac6-220">Använd följande för att hämta och använda dem som en del av skriptet:</span><span class="sxs-lookup"><span data-stu-id="1cac6-220">Use the following to download and use them as part of your script:</span></span>

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

<span data-ttu-id="1cac6-221">Följande hjälpfiler tillgängligt för användning i skriptet:</span><span class="sxs-lookup"><span data-stu-id="1cac6-221">The following helpers available for use in your script:</span></span>

| <span data-ttu-id="1cac6-222">Helper användning</span><span class="sxs-lookup"><span data-stu-id="1cac6-222">Helper usage</span></span> | <span data-ttu-id="1cac6-223">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1cac6-223">Description</span></span> |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |<span data-ttu-id="1cac6-224">Hämtar en fil från käll-URI: N till den angivna sökvägen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-224">Downloads a file from the source URI to the specified file path.</span></span> <span data-ttu-id="1cac6-225">Som standard den inte över en befintlig fil.</span><span class="sxs-lookup"><span data-stu-id="1cac6-225">By default, it does not overwrite an existing file.</span></span> |
| `untar_file TARFILE DESTDIR` |<span data-ttu-id="1cac6-226">Extraherar tar-filen (med hjälp av `-xf`) i målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-226">Extracts a tar file (using `-xf`) to the destination directory.</span></span> |
| `test_is_headnode` |<span data-ttu-id="1cac6-227">Om kördes på en klustrets huvudnod, returnerar 1. annars 0.</span><span class="sxs-lookup"><span data-stu-id="1cac6-227">If ran on a cluster head node, return 1; otherwise, 0.</span></span> |
| `test_is_datanode` |<span data-ttu-id="1cac6-228">Om den aktuella noden är en datanod (worker), returnera 1; annars 0.</span><span class="sxs-lookup"><span data-stu-id="1cac6-228">If the current node is a data (worker) node, return a 1; otherwise, 0.</span></span> |
| `test_is_first_datanode` |<span data-ttu-id="1cac6-229">Om den aktuella noden är den första data (worker) nod (namngivna workernode0) returnerar 1. annars 0.</span><span class="sxs-lookup"><span data-stu-id="1cac6-229">If the current node is the first data (worker) node (named workernode0) return a 1; otherwise, 0.</span></span> |
| `get_headnodes` |<span data-ttu-id="1cac6-230">Returnera fullständigt kvalificerade domännamnet för headnodes i klustret.</span><span class="sxs-lookup"><span data-stu-id="1cac6-230">Return the fully qualified domain name of the headnodes in the cluster.</span></span> <span data-ttu-id="1cac6-231">Namn är kommaavgränsade.</span><span class="sxs-lookup"><span data-stu-id="1cac6-231">Names are comma delimited.</span></span> <span data-ttu-id="1cac6-232">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-232">An empty string is returned on error.</span></span> |
| `get_primary_headnode` |<span data-ttu-id="1cac6-233">Hämtar fullständigt kvalificerade domännamnet för den primära headnode.</span><span class="sxs-lookup"><span data-stu-id="1cac6-233">Gets the fully qualified domain name of the primary headnode.</span></span> <span data-ttu-id="1cac6-234">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-234">An empty string is returned on error.</span></span> |
| `get_secondary_headnode` |<span data-ttu-id="1cac6-235">Hämtar fullständigt kvalificerade domännamnet för den sekundära headnode.</span><span class="sxs-lookup"><span data-stu-id="1cac6-235">Gets the fully qualified domain name of the secondary headnode.</span></span> <span data-ttu-id="1cac6-236">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-236">An empty string is returned on error.</span></span> |
| `get_primary_headnode_number` |<span data-ttu-id="1cac6-237">Hämtar den primära headnode numeriskt suffix.</span><span class="sxs-lookup"><span data-stu-id="1cac6-237">Gets the numeric suffix of the primary headnode.</span></span> <span data-ttu-id="1cac6-238">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-238">An empty string is returned on error.</span></span> |
| `get_secondary_headnode_number` |<span data-ttu-id="1cac6-239">Hämtar den sekundära headnode numeriskt suffix.</span><span class="sxs-lookup"><span data-stu-id="1cac6-239">Gets the numeric suffix of the secondary headnode.</span></span> <span data-ttu-id="1cac6-240">En tom sträng returneras vid fel.</span><span class="sxs-lookup"><span data-stu-id="1cac6-240">An empty string is returned on error.</span></span> |

## <span data-ttu-id="1cac6-241"><a name="commonusage"></a>Vanliga användningsmönster</span><span class="sxs-lookup"><span data-stu-id="1cac6-241"><a name="commonusage"></a>Common usage patterns</span></span>

<span data-ttu-id="1cac6-242">Det här avsnittet ger vägledning om att implementera några av de vanliga användningsmönster som kan uppstå vid skrivning till ett eget skript.</span><span class="sxs-lookup"><span data-stu-id="1cac6-242">This section provides guidance on implementing some of the common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="passing-parameters-to-a-script"></a><span data-ttu-id="1cac6-243">Skicka parametrar till ett skript</span><span class="sxs-lookup"><span data-stu-id="1cac6-243">Passing parameters to a script</span></span>

<span data-ttu-id="1cac6-244">I vissa fall, kan skriptet kräver parametrar.</span><span class="sxs-lookup"><span data-stu-id="1cac6-244">In some cases, your script may require parameters.</span></span> <span data-ttu-id="1cac6-245">Du kan till exempel behöva administratörslösenordet för klustret när du använder Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="1cac6-245">For example, you may need the admin password for the cluster when using the Ambari REST API.</span></span>

<span data-ttu-id="1cac6-246">Parametrar för skriptet kallas *positionsparametrarna*, och tilldelas `$1` för den första parametern `$2` för andra, och så på.</span><span class="sxs-lookup"><span data-stu-id="1cac6-246">Parameters passed to the script are known as *positional parameters*, and are assigned to `$1` for the first parameter, `$2` for the second, and so-on.</span></span> <span data-ttu-id="1cac6-247">`$0`innehåller namnet på själva skriptet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-247">`$0` contains the name of the script itself.</span></span>

<span data-ttu-id="1cac6-248">Värden har överförts till skriptet som parametrar ska omges av enkla citattecken (').</span><span class="sxs-lookup"><span data-stu-id="1cac6-248">Values passed to the script as parameters should be enclosed by single quotes (').</span></span> <span data-ttu-id="1cac6-249">På så sätt att det angivna värdet behandlas som en literal.</span><span class="sxs-lookup"><span data-stu-id="1cac6-249">Doing so ensures that the passed value is treated as a literal.</span></span>

### <a name="setting-environment-variables"></a><span data-ttu-id="1cac6-250">Ställa in miljövariabler</span><span class="sxs-lookup"><span data-stu-id="1cac6-250">Setting environment variables</span></span>

<span data-ttu-id="1cac6-251">Ange en miljövariabel utförs av följande sats:</span><span class="sxs-lookup"><span data-stu-id="1cac6-251">Setting an environment variable is performed by the following statement:</span></span>

    VARIABLENAME=value

<span data-ttu-id="1cac6-252">Där VARIABELNAMN är namnet på variabeln.</span><span class="sxs-lookup"><span data-stu-id="1cac6-252">Where VARIABLENAME is the name of the variable.</span></span> <span data-ttu-id="1cac6-253">Om du vill få åtkomst till variabeln måste använda `$VARIABLENAME`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-253">To access the variable, use `$VARIABLENAME`.</span></span> <span data-ttu-id="1cac6-254">Om du vill tilldela ett värde som tillhandahålls av en positionsparametrarna parameter som miljövariabeln lösenord skulle du till exempel använda följande sats:</span><span class="sxs-lookup"><span data-stu-id="1cac6-254">For example, to assign a value provided by a positional parameter as an environment variable named PASSWORD, you would use the following statement:</span></span>

    PASSWORD=$1

<span data-ttu-id="1cac6-255">Efterföljande åtkomst till informationen kan sedan använda `$PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-255">Subsequent access to the information could then use `$PASSWORD`.</span></span>

<span data-ttu-id="1cac6-256">Miljövariabler som anges i skriptet kan bara finnas inom omfånget för skriptet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-256">Environment variables set within the script only exist within the scope of the script.</span></span> <span data-ttu-id="1cac6-257">I vissa fall kan behöva du lägga till systemomfattande miljövariabler som behålls när skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1cac6-257">In some cases, you may need to add system-wide environment variables that will persist after the script has finished.</span></span> <span data-ttu-id="1cac6-258">Lägga till variabeln för att lägga till systemomfattande miljövariabler `/etc/environment`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-258">To add system-wide environment variables, add the variable to `/etc/environment`.</span></span> <span data-ttu-id="1cac6-259">Till exempel följande uttryck lägger till `HADOOP_CONF_DIR`:</span><span class="sxs-lookup"><span data-stu-id="1cac6-259">For example, the following statement adds `HADOOP_CONF_DIR`:</span></span>

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a><span data-ttu-id="1cac6-260">Åtkomst till platser där anpassade skript lagras</span><span class="sxs-lookup"><span data-stu-id="1cac6-260">Access to locations where the custom scripts are stored</span></span>

<span data-ttu-id="1cac6-261">Skript som används för att anpassa ett kluster måste lagras i något av följande platser:</span><span class="sxs-lookup"><span data-stu-id="1cac6-261">Scripts used to customize a cluster needs to be stored in one of the following locations:</span></span>

* <span data-ttu-id="1cac6-262">En __Azure Storage-konto__ som är associerad med klustret.</span><span class="sxs-lookup"><span data-stu-id="1cac6-262">An __Azure Storage account__ that is associated with the cluster.</span></span>

* <span data-ttu-id="1cac6-263">En __ytterligare lagringskonto__ som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="1cac6-263">An __additional storage account__ associated with the cluster.</span></span>

* <span data-ttu-id="1cac6-264">En __offentligt läsbar URI__.</span><span class="sxs-lookup"><span data-stu-id="1cac6-264">A __publicly readable URI__.</span></span> <span data-ttu-id="1cac6-265">Till exempel en URL till data som lagras på OneDrive, Dropbox eller andra filer som är värd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1cac6-265">For example, a URL to data stored on OneDrive, Dropbox, or other file hosting service.</span></span>

* <span data-ttu-id="1cac6-266">En __Azure Data Lake Store-konto__ som är associerad med HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="1cac6-266">An __Azure Data Lake Store account__ that is associated with the HDInsight cluster.</span></span> <span data-ttu-id="1cac6-267">Mer information om hur du använder Azure Data Lake Store med HDInsight finns [skapar ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1cac6-267">For more information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="1cac6-268">Tjänstens huvudnamn HDInsight använder för åtkomst till Data Lake Store måste ha läsbehörighet till skriptet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-268">The service principal HDInsight uses to access Data Lake Store must have read access to the script.</span></span>

<span data-ttu-id="1cac6-269">Resurser som används av skriptet måste också vara offentligt tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="1cac6-269">Resources used by the script must also be publicly available.</span></span>

<span data-ttu-id="1cac6-270">Lagra filer i en Azure Storage-konto eller ett Azure Data Lake Store ger snabb åtkomst som både i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="1cac6-270">Storing the files in an Azure Storage account or Azure Data Lake Store provides fast access, as both within the Azure network.</span></span>

> [!NOTE]
> <span data-ttu-id="1cac6-271">URI-format som används för att referera till skriptet varierar beroende på vilken tjänst som används.</span><span class="sxs-lookup"><span data-stu-id="1cac6-271">The URI format used to reference the script differs depending on the service being used.</span></span> <span data-ttu-id="1cac6-272">Storage-konton som är associerade med HDInsight-klustret kan använda `wasb://` eller `wasbs://`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-272">For storage accounts associated with the HDInsight cluster, use `wasb://` or `wasbs://`.</span></span> <span data-ttu-id="1cac6-273">Offentligt läsbar URI: er använda `http://` eller `https://`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-273">For publicly readable URIs, use `http://` or `https://`.</span></span> <span data-ttu-id="1cac6-274">Data Lake Store använder `adl://`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-274">For Data Lake Store, use `adl://`.</span></span>

### <a name="checking-the-operating-system-version"></a><span data-ttu-id="1cac6-275">Kontrollerar operativsystemets version</span><span class="sxs-lookup"><span data-stu-id="1cac6-275">Checking the operating system version</span></span>

<span data-ttu-id="1cac6-276">Olika versioner av HDInsight är beroende av specifika versioner av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1cac6-276">Different versions of HDInsight rely on specific versions of Ubuntu.</span></span> <span data-ttu-id="1cac6-277">Det kan finnas skillnader mellan OS-versioner måste du kontrollera i skriptet.</span><span class="sxs-lookup"><span data-stu-id="1cac6-277">There may be differences between OS versions that you must check for in your script.</span></span> <span data-ttu-id="1cac6-278">Du kan behöva installera en binärfil som är kopplade till versionen av Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1cac6-278">For example, you may need to install a binary that is tied to the version of Ubuntu.</span></span>

<span data-ttu-id="1cac6-279">För att kontrollera versionen av Operativsystemet, använda `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-279">To check the OS version, use `lsb_release`.</span></span> <span data-ttu-id="1cac6-280">Till exempel visar följande skript hur du refererar till en specifik tar-filen beroende på OS-version:</span><span class="sxs-lookup"><span data-stu-id="1cac6-280">For example, the following script demonstrates how to reference a specific tar file depending on the OS version:</span></span>

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

## <span data-ttu-id="1cac6-281"><a name="deployScript"></a>Checklista för distribution av en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="1cac6-281"><a name="deployScript"></a>Checklist for deploying a script action</span></span>

<span data-ttu-id="1cac6-282">Här följer de steg som vi har tagit när du förbereder att distribuera dessa skript:</span><span class="sxs-lookup"><span data-stu-id="1cac6-282">Here are the steps we took when preparing to deploy these scripts:</span></span>

* <span data-ttu-id="1cac6-283">Placera de filer som innehåller anpassade skript på en plats som kan nås av klusternoder under distributionen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-283">Put the files that contain the custom scripts in a place that is accessible by the cluster nodes during deployment.</span></span> <span data-ttu-id="1cac6-284">Till exempel standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="1cac6-284">For example, the default storage for the cluster.</span></span> <span data-ttu-id="1cac6-285">Filer kan också lagras i offentligt läsbar värdtjänster.</span><span class="sxs-lookup"><span data-stu-id="1cac6-285">Files can also be stored in publicly readable hosting services.</span></span>
* <span data-ttu-id="1cac6-286">Kontrollera att skriptet är impotent.</span><span class="sxs-lookup"><span data-stu-id="1cac6-286">Verify that the script is impotent.</span></span> <span data-ttu-id="1cac6-287">På så sätt kan skriptet ska köras flera gånger på samma nod.</span><span class="sxs-lookup"><span data-stu-id="1cac6-287">Doing so allows the script to be executed multiple times on the same node.</span></span>
* <span data-ttu-id="1cac6-288">Använd en tillfällig katalog /tmp för att hålla de hämtade filer som används av skripten och sedan rensa dem efter skript har körts.</span><span class="sxs-lookup"><span data-stu-id="1cac6-288">Use a temporary file directory /tmp to keep the downloaded files used by the scripts and then clean them up after scripts have executed.</span></span>
* <span data-ttu-id="1cac6-289">Om inställningar för OS-nivå eller konfigurationsfiler för Hadoop-tjänsten ändras, kan du vill starta om HDInsight-tjänster.</span><span class="sxs-lookup"><span data-stu-id="1cac6-289">If OS-level settings or Hadoop service configuration files are changed, you may want to restart HDInsight services.</span></span>

## <span data-ttu-id="1cac6-290"><a name="runScriptAction"></a>Så här kör du en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="1cac6-290"><a name="runScriptAction"></a>How to run a script action</span></span>

<span data-ttu-id="1cac6-291">Använd skriptåtgärder om du vill anpassa HDInsight-kluster med följande metoder:</span><span class="sxs-lookup"><span data-stu-id="1cac6-291">You can use script actions to customize HDInsight clusters using the following methods:</span></span>

* <span data-ttu-id="1cac6-292">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1cac6-292">Azure portal</span></span>
* <span data-ttu-id="1cac6-293">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1cac6-293">Azure PowerShell</span></span>
* <span data-ttu-id="1cac6-294">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1cac6-294">Azure Resource Manager templates</span></span>
* <span data-ttu-id="1cac6-295">HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="1cac6-295">The HDInsight .NET SDK.</span></span>

<span data-ttu-id="1cac6-296">Mer information om hur du använder varje metod finns [hur du använder skriptåtgärd](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1cac6-296">For more information on using each method, see [How to use script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="1cac6-297"><a name="sampleScripts"></a>Anpassade skriptexempel</span><span class="sxs-lookup"><span data-stu-id="1cac6-297"><a name="sampleScripts"></a>Custom script samples</span></span>

<span data-ttu-id="1cac6-298">Microsoft tillhandahåller exempelskript för att installera komponenterna på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1cac6-298">Microsoft provides sample scripts to install components on an HDInsight cluster.</span></span> <span data-ttu-id="1cac6-299">Se följande länkar för flera exempel skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1cac6-299">See the following links for more example script actions.</span></span>

* [<span data-ttu-id="1cac6-300">Installera och använda Hue på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="1cac6-300">Install and use Hue on HDInsight clusters</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="1cac6-301">Installera och använda Solr på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="1cac6-301">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="1cac6-302">Installera och använda Giraph på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="1cac6-302">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="1cac6-303">Installera eller uppgradera Mono på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="1cac6-303">Install or upgrade Mono on HDInsight clusters</span></span>](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a><span data-ttu-id="1cac6-304">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1cac6-304">Troubleshooting</span></span>

<span data-ttu-id="1cac6-305">Följande är fel som kan uppstå när du använder skript som du har utvecklat:</span><span class="sxs-lookup"><span data-stu-id="1cac6-305">The following are errors you may encounter when using scripts you have developed:</span></span>

<span data-ttu-id="1cac6-306">**Fel**: `$'\r': command not found`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-306">**Error**: `$'\r': command not found`.</span></span> <span data-ttu-id="1cac6-307">Ibland följt av `syntax error: unexpected end of file`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-307">Sometimes followed by `syntax error: unexpected end of file`.</span></span>

<span data-ttu-id="1cac6-308">*Orsak*: det här felet uppstår om raderna i ett skript som avslutas med CRLF.</span><span class="sxs-lookup"><span data-stu-id="1cac6-308">*Cause*: This error is caused when the lines in a script end with CRLF.</span></span> <span data-ttu-id="1cac6-309">UNIX-system förväntar sig endast LF som rad avslutas.</span><span class="sxs-lookup"><span data-stu-id="1cac6-309">Unix systems expect only LF as the line ending.</span></span>

<span data-ttu-id="1cac6-310">Det här problemet inträffar oftast när skriptet har skapats på en Windows-miljö som CRLF är en gemensam rader som slutar med för många textredigerare i Windows.</span><span class="sxs-lookup"><span data-stu-id="1cac6-310">This problem most often occurs when the script is authored on a Windows environment, as CRLF is a common line ending for many text editors on Windows.</span></span>

<span data-ttu-id="1cac6-311">*Lösning*: om det är ett alternativ i en textredigerare, Välj Unix-format eller LF för raden avslutas.</span><span class="sxs-lookup"><span data-stu-id="1cac6-311">*Resolution*: If it is an option in your text editor, select Unix format or LF for the line ending.</span></span> <span data-ttu-id="1cac6-312">Du kan även använda följande kommandon på ett Unix-system för att ändra CRLF till en LF:</span><span class="sxs-lookup"><span data-stu-id="1cac6-312">You may also use the following commands on a Unix system to change the CRLF to an LF:</span></span>

> [!NOTE]
> <span data-ttu-id="1cac6-313">Följande kommandon är ungefär eftersom de bör ändra radbrytningar CRLF till LF.</span><span class="sxs-lookup"><span data-stu-id="1cac6-313">The following commands are roughly equivalent in that they should change the CRLF line endings to LF.</span></span> <span data-ttu-id="1cac6-314">Välj en baserat på Verktyg som finns på datorn.</span><span class="sxs-lookup"><span data-stu-id="1cac6-314">Select one based on the utilities available on your system.</span></span>

| <span data-ttu-id="1cac6-315">Kommando</span><span class="sxs-lookup"><span data-stu-id="1cac6-315">Command</span></span> | <span data-ttu-id="1cac6-316">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1cac6-316">Notes</span></span> |
| --- | --- |
| `unix2dos -b INFILE` |<span data-ttu-id="1cac6-317">Den ursprungliga filen säkerhetskopieras med en. BAK-tillägg</span><span class="sxs-lookup"><span data-stu-id="1cac6-317">The original file is backed up with a .BAK extension</span></span> |
| `tr -d '\r' < INFILE > OUTFILE` |<span data-ttu-id="1cac6-318">UTFIL innehåller en version med endast LF ändelser</span><span class="sxs-lookup"><span data-stu-id="1cac6-318">OUTFILE contains a version with only LF endings</span></span> |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | <span data-ttu-id="1cac6-319">Ändrar filen direkt</span><span class="sxs-lookup"><span data-stu-id="1cac6-319">Modifies the file directly</span></span> |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |<span data-ttu-id="1cac6-320">UTFIL innehåller en version med endast LF ändelser.</span><span class="sxs-lookup"><span data-stu-id="1cac6-320">OUTFILE contains a version with only LF endings.</span></span> |

<span data-ttu-id="1cac6-321">**Fel**: `line 1: #!/usr/bin/env: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="1cac6-321">**Error**: `line 1: #!/usr/bin/env: No such file or directory`.</span></span>

<span data-ttu-id="1cac6-322">*Orsak*: det här felet uppstår när skriptet har sparats som UTF-8 en Byte ordning markering (BOM).</span><span class="sxs-lookup"><span data-stu-id="1cac6-322">*Cause*: This error occurs when the script was saved as UTF-8 with a Byte Order Mark (BOM).</span></span>

<span data-ttu-id="1cac6-323">*Lösning*: spara filen som ASCII eller som UTF-8 utan en struktur.</span><span class="sxs-lookup"><span data-stu-id="1cac6-323">*Resolution*: Save the file either as ASCII, or as UTF-8 without a BOM.</span></span> <span data-ttu-id="1cac6-324">Du kan också använda följande kommando på en Linux eller Unix-system för att skapa en fil utan Strukturen:</span><span class="sxs-lookup"><span data-stu-id="1cac6-324">You may also use the following command on a Linux or Unix system to create a file without the BOM:</span></span>

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

<span data-ttu-id="1cac6-325">Ersätt `INFILE` med den fil som innehåller Strukturen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-325">Replace `INFILE` with the file containing the BOM.</span></span> <span data-ttu-id="1cac6-326">`OUTFILE`måste vara ett nytt filnamn som innehåller skriptet utan Strukturen.</span><span class="sxs-lookup"><span data-stu-id="1cac6-326">`OUTFILE` should be a new file name, which contains the script without the BOM.</span></span>

## <span data-ttu-id="1cac6-327"><a name="seeAlso"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1cac6-327"><a name="seeAlso"></a>Next steps</span></span>

* <span data-ttu-id="1cac6-328">Lär dig hur du [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="1cac6-328">Learn how to [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
* <span data-ttu-id="1cac6-329">Använd den [HDInsight .NET SDK referens](https://msdn.microsoft.com/library/mt271028.aspx) vill veta mer om hur du skapar .NET-program som hanterar HDInsight</span><span class="sxs-lookup"><span data-stu-id="1cac6-329">Use the [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx) to learn more about creating .NET applications that manage HDInsight</span></span>
* <span data-ttu-id="1cac6-330">Använd den [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) att lära dig hur du använder REST för att utföra hanteringsåtgärder på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1cac6-330">Use the [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) to learn how to use REST to perform management actions on HDInsight clusters.</span></span>
