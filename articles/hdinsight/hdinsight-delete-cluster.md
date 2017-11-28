---
title: aaaHow toodelete ett HDInsight-kluster - Azure | Microsoft Docs
description: "Information om hello olika sätt att du tar bort ett HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 5b9d9a09eecfdcfaed7a1f5ebab440e13bd358b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-hello-azure-cli"></a><span data-ttu-id="33e40-103">Ta bort ett HDInsight-kluster med hjälp av din webbläsare, PowerShell eller hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="33e40-103">Delete an HDInsight cluster using your browser, PowerShell, or hello Azure CLI</span></span>

<span data-ttu-id="33e40-104">HDInsight-kluster faktureringen påbörjas så snart ett kluster skapas och stoppas när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="33e40-104">HDInsight cluster billing starts once a cluster is created and stops when hello cluster is deleted.</span></span> <span data-ttu-id="33e40-105">Fakturering är proportionerligt per minut, så du bör alltid ta bort klustret när den inte längre används.</span><span class="sxs-lookup"><span data-stu-id="33e40-105">Billing is pro-rated per minute, so you should always delete your cluster when it is no longer in use.</span></span> <span data-ttu-id="33e40-106">I detta dokument kan du lära dig hur toodelete ett kluster som använder hello Azure-portalen, Azure PowerShell och hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="33e40-106">In this document, you learn how toodelete a cluster using hello Azure portal, Azure PowerShell, and hello Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33e40-107">Tar bort ett HDInsight-kluster tas inte bort hello Azure Storage-konton eller som är associerade med klustret hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="33e40-107">Deleting an HDInsight cluster does not delete hello Azure Storage accounts or Data Lake Store associated with hello cluster.</span></span> <span data-ttu-id="33e40-108">Du kan återanvända data som lagras i dessa tjänster i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="33e40-108">You can reuse data stored in those services in hello future.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="33e40-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33e40-109">Azure portal</span></span>

1. <span data-ttu-id="33e40-110">Logga in toohello [Azure-portalen](https://portal.azure.com) och välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="33e40-110">Log in toohello [Azure portal](https://portal.azure.com) and select your HDInsight cluster.</span></span> <span data-ttu-id="33e40-111">Om ditt HDInsight-kluster inte är fäst toohello instrumentpanelen, kan du söka efter den efter namn med hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="33e40-111">If your HDInsight cluster is not pinned toohello dashboard, you can search for it by name using hello search field.</span></span>
   
    ![Portal-sökning](./media/hdinsight-delete-cluster/navbar.png)

2. <span data-ttu-id="33e40-113">När hello blad öppnas för hello kluster, Välj hello **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="33e40-113">Once hello blade opens for hello cluster, select hello **Delete** icon.</span></span> <span data-ttu-id="33e40-114">När du uppmanas, Välj **Ja** toodelete hello klustret.</span><span class="sxs-lookup"><span data-stu-id="33e40-114">When prompted, select **Yes** toodelete hello cluster.</span></span>
   
    ![Ikonen Ta bort](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a><span data-ttu-id="33e40-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="33e40-116">Azure PowerShell</span></span>

<span data-ttu-id="33e40-117">Använd hello efter kommandot toodelete hello kluster från en PowerShell-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="33e40-117">From a PowerShell prompt, use hello following command toodelete hello cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

<span data-ttu-id="33e40-118">Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="33e40-118">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

## <a name="azure-cli-10"></a><span data-ttu-id="33e40-119">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="33e40-119">Azure CLI 1.0</span></span>

<span data-ttu-id="33e40-120">Använd hello följande toodelete hello kluster från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="33e40-120">From a prompt, use hello following toodelete hello cluster:</span></span>

    azure hdinsight cluster delete CLUSTERNAME

<span data-ttu-id="33e40-121">Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="33e40-121">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="33e40-122">Azure CLI 2.0 stöder inte ta bort HDInsight-kluster just nu (31 juli 2017).</span><span class="sxs-lookup"><span data-stu-id="33e40-122">Azure CLI 2.0 does not support deleting HDInsight clusters at this time (July 31, 2017).</span></span>