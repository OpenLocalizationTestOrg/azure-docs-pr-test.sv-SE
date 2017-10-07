---
title: "Hadoop-aaaManage kluster med hjälp av Azure CLI - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hello Azure-kommandoradsgränssnittet toomanage Hadoop-kluster i Azure HDInsight. hello Azure CLI fungerar i Windows, Mac- och Linux."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 03b0cff9331c1c581095b80cc6d1177d843ffa83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-hello-azure-cli"></a><span data-ttu-id="70a19-104">Hantera Hadoop-kluster i HDInsight med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="70a19-104">Manage Hadoop clusters in HDInsight using hello Azure CLI</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="70a19-105">Lär dig hur toouse hello [Azure-kommandoradsgränssnittet](../cli-install-nodejs.md) toomanage Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="70a19-105">Learn how toouse hello [Azure Command-line Interface](../cli-install-nodejs.md) toomanage Hadoop clusters in Azure HDInsight.</span></span> <span data-ttu-id="70a19-106">hello Azure CLI är implementerat i Node.js.</span><span class="sxs-lookup"><span data-stu-id="70a19-106">hello Azure CLI is implemented in Node.js.</span></span> <span data-ttu-id="70a19-107">Det kan användas på alla plattformar som har stöd för Node.js, inklusive Windows, Mac- och Linux.</span><span class="sxs-lookup"><span data-stu-id="70a19-107">It can be used on any platform that supports Node.js, including Windows, Mac, and Linux.</span></span>

<span data-ttu-id="70a19-108">Den här artikeln täcker endast hello Azure CLI med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="70a19-108">This article covers only using hello Azure CLI with HDInsight.</span></span> <span data-ttu-id="70a19-109">En allmän vägledning om hur toouse Azure CLI, se [installera och konfigurera Azure CLI][azure-command-line-tools].</span><span class="sxs-lookup"><span data-stu-id="70a19-109">For a general guide on how toouse Azure CLI, see [Install and configure Azure CLI][azure-command-line-tools].</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a><span data-ttu-id="70a19-110">Krav</span><span class="sxs-lookup"><span data-stu-id="70a19-110">Prerequisites</span></span>
<span data-ttu-id="70a19-111">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="70a19-111">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="70a19-112">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="70a19-112">**An Azure subscription**.</span></span> <span data-ttu-id="70a19-113">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="70a19-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="70a19-114">**Azure CLI** -finns [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) information om installation och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="70a19-114">**Azure CLI** - See [Install and configure hello Azure CLI](../cli-install-nodejs.md) for installation and configuration information.</span></span>
* <span data-ttu-id="70a19-115">**Ansluta tooAzure**med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="70a19-115">**Connect tooAzure**, using hello following command:</span></span>
  
        azure login
  
    <span data-ttu-id="70a19-116">Mer information om autentisering med ett arbets- eller skolkonto konto finns [ansluta tooan Azure-prenumeration från hello Azure CLI](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="70a19-116">For more information on authenticating using a work or school account, see [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="70a19-117">**Växla toohello Azure Resource Manager läge**med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="70a19-117">**Switch toohello Azure Resource Manager mode**, using hello following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="70a19-118">tooget hjälp, använda hello **-h** växla.</span><span class="sxs-lookup"><span data-stu-id="70a19-118">tooget help, use hello **-h** switch.</span></span>  <span data-ttu-id="70a19-119">Exempel:</span><span class="sxs-lookup"><span data-stu-id="70a19-119">For example:</span></span>

    azure hdinsight cluster create -h

## <a name="create-clusters-with-hello-cli"></a><span data-ttu-id="70a19-120">Skapa kluster med hello CLI</span><span class="sxs-lookup"><span data-stu-id="70a19-120">Create clusters with hello CLI</span></span>
<span data-ttu-id="70a19-121">Se [Skapa kluster i HDInsight med hello Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="70a19-121">See [Create clusters in HDInsight using hello Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span></span>

## <a name="list-and-show-cluster-details"></a><span data-ttu-id="70a19-122">Listan och visa information om kluster</span><span class="sxs-lookup"><span data-stu-id="70a19-122">List and show cluster details</span></span>
<span data-ttu-id="70a19-123">Använd följande kommandon toolist hello och visa information om kluster:</span><span class="sxs-lookup"><span data-stu-id="70a19-123">Use hello following commands toolist and show cluster details:</span></span>

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

<span data-ttu-id="70a19-124">![Kommandoradsverktyget vy av klustret lista][image-cli-clusterlisting]</span><span class="sxs-lookup"><span data-stu-id="70a19-124">![Command-line view of cluster list][image-cli-clusterlisting]</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="70a19-125">Ta bort kluster</span><span class="sxs-lookup"><span data-stu-id="70a19-125">Delete clusters</span></span>
<span data-ttu-id="70a19-126">Använd följande kommando toodelete ett kluster hello:</span><span class="sxs-lookup"><span data-stu-id="70a19-126">Use hello following command toodelete a cluster:</span></span>

    azure hdinsight cluster delete <Cluster Name>

<span data-ttu-id="70a19-127">Du kan också ta bort ett kluster genom att ta bort hello resursgruppen som innehåller hello klustret.</span><span class="sxs-lookup"><span data-stu-id="70a19-127">You can also delete a cluster by deleting hello resource group that contains hello cluster.</span></span> <span data-ttu-id="70a19-128">Observera detta tar bort alla hello resurser i hello grupp, inklusive hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="70a19-128">Please note, this will delete all hello resources in hello group including hello default storage account.</span></span>

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="70a19-129">Skala kluster</span><span class="sxs-lookup"><span data-stu-id="70a19-129">Scale clusters</span></span>
<span data-ttu-id="70a19-130">toochange hello Hadoop klusterstorleken:</span><span class="sxs-lookup"><span data-stu-id="70a19-130">toochange hello Hadoop cluster size:</span></span>

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a><span data-ttu-id="70a19-131">Aktivera/inaktivera HTTP-åtkomst för ett kluster</span><span class="sxs-lookup"><span data-stu-id="70a19-131">Enable/disable HTTP access for a cluster</span></span>
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a><span data-ttu-id="70a19-132">Aktivera/inaktivera RDP-åtkomst för ett kluster</span><span class="sxs-lookup"><span data-stu-id="70a19-132">Enable/disable RDP access for a cluster</span></span>
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a><span data-ttu-id="70a19-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70a19-133">Next steps</span></span>
<span data-ttu-id="70a19-134">I den här artikeln har du lärt dig hur tooperform olika HDInsight-kluster administrativa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="70a19-134">In this article, you have learned how tooperform different HDInsight cluster administrative tasks.</span></span> <span data-ttu-id="70a19-135">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="70a19-135">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="70a19-136">[Administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="70a19-136">[Administer HDInsight by using hello Azure Portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="70a19-137">[Administrera HDInsight med hjälp av Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="70a19-137">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="70a19-138">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="70a19-138">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="70a19-139">[Hur toouse hello Azure CLI][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="70a19-139">[How toouse hello Azure CLI][azure-command-line-tools]</span></span>

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Listan och visa kluster"
