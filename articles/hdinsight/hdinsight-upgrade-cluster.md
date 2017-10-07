---
title: aaaUpgrade HDInsight-kluster tooa nyare version-Azure | Microsoft Docs
description: "Lär dig hur tooUpgrade HDInsight-kluster tooa nyare version."
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
ms.openlocfilehash: 5fff3c9bc88dfbcbc1ccb0188accdfbbec3a62f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hdinsight-cluster-tooa-newer-version"></a><span data-ttu-id="af018-103">HDInsight-kluster tooa nyare uppgraderingsversion</span><span class="sxs-lookup"><span data-stu-id="af018-103">Upgrade HDInsight cluster tooa newer version</span></span>
<span data-ttu-id="af018-104">tootake nytta av Hej senaste HDInsight-funktionerna, rekommenderar vi att uppgraderade toolatest version på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="af018-104">tootake advantage of hello latest HDInsight features, we recommend that HDInsight clusters be upgraded toolatest version.</span></span> <span data-ttu-id="af018-105">Följ hello nedan riktlinjer tooupgrade HDInsight-kluster-versioner.</span><span class="sxs-lookup"><span data-stu-id="af018-105">Follow hello below guidelines tooupgrade your HDInsight cluster versions.</span></span>

> [!NOTE]
> <span data-ttu-id="af018-106">HDInsight-kluster version 3.2 eller 3.3 snart datumet för tillbakadragandet.</span><span class="sxs-lookup"><span data-stu-id="af018-106">HDInsight clusters version 3.2 and 3.3 are nearing retirement date.</span></span> <span data-ttu-id="af018-107">Information om HDInsight version som stöds finns [HDInsight komponenten versioner](hdinsight-component-versioning.md#supported-hdinsight-versions).</span><span class="sxs-lookup"><span data-stu-id="af018-107">For information on supported version of HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md#supported-hdinsight-versions).</span></span>
>
>

## <a name="upgrade-tasks"></a><span data-ttu-id="af018-108">Uppgraderingsuppgifter</span><span class="sxs-lookup"><span data-stu-id="af018-108">Upgrade tasks</span></span>
<span data-ttu-id="af018-109">hello arbetsflöde tooupgrade HDInsight-kluster är som följer.</span><span class="sxs-lookup"><span data-stu-id="af018-109">hello workflow tooupgrade HDInsight Cluster is as follows.</span></span>

![Uppgradera arbetsflödesdiagram](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. <span data-ttu-id="af018-111">Läs avsnitten i det här dokumentet toounderstand ändringar som kan krävas när du uppgraderar ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="af018-111">Read each section of this document toounderstand changes that may be required when upgrading your HDInsight cluster.</span></span>
2. <span data-ttu-id="af018-112">Skapa ett kluster som en test/kvalitet försäkran miljö.</span><span class="sxs-lookup"><span data-stu-id="af018-112">Create a cluster as a test/quality assurance environment.</span></span> <span data-ttu-id="af018-113">Mer information om hur du skapar ett kluster finns [Lär dig hur toocreate Linux-baserade HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="af018-113">For more information on creating a cluster, see [Learn how toocreate Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
3. <span data-ttu-id="af018-114">Kopiera befintliga jobb, datakällor, och egenskaperna toohello nya miljön.</span><span class="sxs-lookup"><span data-stu-id="af018-114">Copy existing jobs, data sources, and sinks toohello new environment.</span></span> <span data-ttu-id="af018-115">Se [kopieringsdata tooTest miljö](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) för mer information.</span><span class="sxs-lookup"><span data-stu-id="af018-115">See [Copy Data tooTest Environment](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) for more details.</span></span>
4. <span data-ttu-id="af018-116">Verifieringen testning toomake till att dina jobb fungerar som förväntat på hello nya klustret.</span><span class="sxs-lookup"><span data-stu-id="af018-116">Perform validation testing toomake sure that your jobs work as expected on hello new cluster.</span></span>


<span data-ttu-id="af018-117">När du har kontrollerat att allt fungerar som förväntat, Schemalägg driftstopp för hello migrering.</span><span class="sxs-lookup"><span data-stu-id="af018-117">Once you have verified that everything works as expected, schedule downtime for hello migration.</span></span> <span data-ttu-id="af018-118">Under den här driftstörningen hello gör du följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="af018-118">During this downtime, do hello following actions:</span></span>

1.  <span data-ttu-id="af018-119">Säkerhetskopiera alla tillfälligt data som lagras lokalt på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="af018-119">Back up any transient data stored locally on hello cluster nodes.</span></span> <span data-ttu-id="af018-120">Till exempel om du har data som lagras direkt på en huvudnod.</span><span class="sxs-lookup"><span data-stu-id="af018-120">For example, if you have data stored directly on a head node.</span></span>
2.  <span data-ttu-id="af018-121">Ta bort hello befintligt kluster.</span><span class="sxs-lookup"><span data-stu-id="af018-121">Delete hello existing cluster.</span></span>
3.  <span data-ttu-id="af018-122">Skapa ett kluster i hello samma virtuella nätverk undernät med senaste (eller stöds) HDI version med hello samma standard datalager som hello tidigare kluster används.</span><span class="sxs-lookup"><span data-stu-id="af018-122">Create a cluster in hello same VNET subnet with latest (or supported) HDI version using hello same default data store that hello previous cluster used.</span></span> <span data-ttu-id="af018-123">Detta gör att hello nya kluster toocontinue arbeta mot ditt befintliga produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="af018-123">This allows hello new cluster toocontinue working against your existing production data.</span></span>
4.  <span data-ttu-id="af018-124">Importera alla tillfälligt säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="af018-124">Import any transient data you backed up.</span></span>
5.  <span data-ttu-id="af018-125">Starta jobb/fortsätta att bearbeta med hello nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="af018-125">Start jobs/continue processing using hello new cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af018-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af018-126">Next Steps</span></span>
* [<span data-ttu-id="af018-127">Lär dig hur toocreate Linux-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="af018-127">Learn how toocreate Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="af018-128">Ansluta tooHDInsight via SSH</span><span class="sxs-lookup"><span data-stu-id="af018-128">Connect tooHDInsight using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)
* [<span data-ttu-id="af018-129">Hantera en Linux-baserade kluster med Ambari</span><span class="sxs-lookup"><span data-stu-id="af018-129">Manage a Linux-based cluster using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

