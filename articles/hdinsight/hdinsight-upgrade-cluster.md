---
title: Uppgradera HDInsight-kluster till en nyare version-Azure | Microsoft Docs
description: "Lär dig hur du uppgraderar HDInsight-kluster till en nyare version."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: fa2e37bd922690322ccc3d8f68128180d013b701
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a><span data-ttu-id="93694-103">Uppgradera HDInsight-kluster till en nyare version</span><span class="sxs-lookup"><span data-stu-id="93694-103">Upgrade HDInsight cluster to a newer version</span></span>
<span data-ttu-id="93694-104">Om du vill dra nytta av de senaste funktionerna för HDInsight, rekommenderar vi att HDInsight-kluster uppgraderas till den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="93694-104">To take advantage of the latest HDInsight features, we recommend that HDInsight clusters be upgraded to latest version.</span></span> <span data-ttu-id="93694-105">Följ den nedan riktlinjer för att uppgradera ditt HDInsight-kluster versioner.</span><span class="sxs-lookup"><span data-stu-id="93694-105">Follow the below guidelines to upgrade your HDInsight cluster versions.</span></span>

> [!NOTE]
> <span data-ttu-id="93694-106">HDInsight-kluster version 3.2 eller 3.3 snart datumet för tillbakadragandet.</span><span class="sxs-lookup"><span data-stu-id="93694-106">HDInsight clusters version 3.2 and 3.3 are nearing retirement date.</span></span> <span data-ttu-id="93694-107">Information om HDInsight version som stöds finns [HDInsight komponenten versioner](hdinsight-component-versioning.md#supported-hdinsight-versions).</span><span class="sxs-lookup"><span data-stu-id="93694-107">For information on supported version of HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md#supported-hdinsight-versions).</span></span>
>
>

## <a name="upgrade-tasks"></a><span data-ttu-id="93694-108">Uppgraderingsuppgifter</span><span class="sxs-lookup"><span data-stu-id="93694-108">Upgrade tasks</span></span>
<span data-ttu-id="93694-109">Arbetsflöde för att uppgradera HDInsight-kluster är som följer.</span><span class="sxs-lookup"><span data-stu-id="93694-109">The workflow to upgrade HDInsight Cluster is as follows.</span></span>

![Uppgradera arbetsflödesdiagram](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. <span data-ttu-id="93694-111">Läs avsnitten i det här dokumentet att förstå ändringar som kan krävas när du uppgraderar ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="93694-111">Read each section of this document to understand changes that may be required when upgrading your HDInsight cluster.</span></span>
2. <span data-ttu-id="93694-112">Skapa ett kluster som en test/kvalitet försäkran miljö.</span><span class="sxs-lookup"><span data-stu-id="93694-112">Create a cluster as a test/quality assurance environment.</span></span> <span data-ttu-id="93694-113">Mer information om hur du skapar ett kluster finns [Lär dig att skapa Linux-baserade HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="93694-113">For more information on creating a cluster, see [Learn how to create Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
3. <span data-ttu-id="93694-114">Kopiera befintliga jobb, datakällor och sänkor till den nya miljön.</span><span class="sxs-lookup"><span data-stu-id="93694-114">Copy existing jobs, data sources, and sinks to the new environment.</span></span> <span data-ttu-id="93694-115">Se [kopiera Data till testmiljö](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) för mer information.</span><span class="sxs-lookup"><span data-stu-id="93694-115">See [Copy Data To Test Environment](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) for more details.</span></span>
4. <span data-ttu-id="93694-116">Utföra verifiering tester för att se till att dina jobb fungerar som förväntat på det nya klustret.</span><span class="sxs-lookup"><span data-stu-id="93694-116">Perform validation testing to make sure that your jobs work as expected on the new cluster.</span></span>


<span data-ttu-id="93694-117">När du har kontrollerat att allt fungerar som förväntat, schemalägga avbrottstiden för migreringen.</span><span class="sxs-lookup"><span data-stu-id="93694-117">Once you have verified that everything works as expected, schedule downtime for the migration.</span></span> <span data-ttu-id="93694-118">Under den här driftstörningen gör du följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="93694-118">During this downtime, do the following actions:</span></span>

1.  <span data-ttu-id="93694-119">Säkerhetskopiera alla tillfälligt data som lagras lokalt på klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="93694-119">Back up any transient data stored locally on the cluster nodes.</span></span> <span data-ttu-id="93694-120">Till exempel om du har data som lagras direkt på en huvudnod.</span><span class="sxs-lookup"><span data-stu-id="93694-120">For example, if you have data stored directly on a head node.</span></span>
2.  <span data-ttu-id="93694-121">Ta bort det befintliga klustret.</span><span class="sxs-lookup"><span data-stu-id="93694-121">Delete the existing cluster.</span></span>
3.  <span data-ttu-id="93694-122">Skapa ett kluster i samma undernät för virtuellt nätverk med senaste (eller stöds) HDI-version med samma standard datalager som används för tidigare kluster.</span><span class="sxs-lookup"><span data-stu-id="93694-122">Create a cluster in the same VNET subnet with latest (or supported) HDI version using the same default data store that the previous cluster used.</span></span> <span data-ttu-id="93694-123">Detta gör det nya klustret kan fortsätta arbeta mot ditt befintliga produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="93694-123">This allows the new cluster to continue working against your existing production data.</span></span>
4.  <span data-ttu-id="93694-124">Importera alla tillfälligt säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="93694-124">Import any transient data you backed up.</span></span>
5.  <span data-ttu-id="93694-125">Starta jobb/fortsätta att bearbeta med det nya klustret.</span><span class="sxs-lookup"><span data-stu-id="93694-125">Start jobs/continue processing using the new cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93694-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93694-126">Next Steps</span></span>
* [<span data-ttu-id="93694-127">Lär dig att skapa Linux-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="93694-127">Learn how to create Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="93694-128">Ansluta till HDInsight med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="93694-128">Connect to HDInsight using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)
* [<span data-ttu-id="93694-129">Hantera en Linux-baserade kluster med Ambari</span><span class="sxs-lookup"><span data-stu-id="93694-129">Manage a Linux-based cluster using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

