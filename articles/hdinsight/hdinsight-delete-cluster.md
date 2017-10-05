---
title: Hur du tar bort ett HDInsight-kluster - Azure | Microsoft Docs
description: "Information om de olika sätt att du tar bort ett HDInsight-kluster."
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
ms.openlocfilehash: 65dac529df15d2dd43eec17673d82a2832f7692e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a><span data-ttu-id="e15c6-103">Ta bort ett HDInsight-kluster med hjälp av din webbläsare, PowerShell eller Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e15c6-103">Delete an HDInsight cluster using your browser, PowerShell, or the Azure CLI</span></span>

<span data-ttu-id="e15c6-104">HDInsight-kluster faktureringen påbörjas när ett kluster skapas och slutar när klustret har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="e15c6-104">HDInsight cluster billing starts once a cluster is created and stops when the cluster is deleted.</span></span> <span data-ttu-id="e15c6-105">Fakturering är proportionerligt per minut, så du bör alltid ta bort klustret när den inte längre används.</span><span class="sxs-lookup"><span data-stu-id="e15c6-105">Billing is pro-rated per minute, so you should always delete your cluster when it is no longer in use.</span></span> <span data-ttu-id="e15c6-106">Lär dig hur du tar bort ett kluster med Azure-portalen, Azure PowerShell och Azure CLI 1.0 i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e15c6-106">In this document, you learn how to delete a cluster using the Azure portal, Azure PowerShell, and the Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e15c6-107">Tar bort ett HDInsight-kluster tas inte bort Azure Storage-konton eller Datasjölager som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="e15c6-107">Deleting an HDInsight cluster does not delete the Azure Storage accounts or Data Lake Store associated with the cluster.</span></span> <span data-ttu-id="e15c6-108">Du kan återanvända data som lagras i dessa tjänster i framtiden.</span><span class="sxs-lookup"><span data-stu-id="e15c6-108">You can reuse data stored in those services in the future.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="e15c6-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e15c6-109">Azure portal</span></span>

1. <span data-ttu-id="e15c6-110">Logga in på den [Azure-portalen](https://portal.azure.com) och välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e15c6-110">Log in to the [Azure portal](https://portal.azure.com) and select your HDInsight cluster.</span></span> <span data-ttu-id="e15c6-111">Om ditt HDInsight-kluster inte är fäst på instrumentpanelen, kan du söka efter den av namn med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="e15c6-111">If your HDInsight cluster is not pinned to the dashboard, you can search for it by name using the search field.</span></span>
   
    ![Portal-sökning](./media/hdinsight-delete-cluster/navbar.png)

2. <span data-ttu-id="e15c6-113">När det öppnas bladet för klustret, väljer du den **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="e15c6-113">Once the blade opens for the cluster, select the **Delete** icon.</span></span> <span data-ttu-id="e15c6-114">När du uppmanas, Välj **Ja** att ta bort klustret.</span><span class="sxs-lookup"><span data-stu-id="e15c6-114">When prompted, select **Yes** to delete the cluster.</span></span>
   
    ![Ikonen Ta bort](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a><span data-ttu-id="e15c6-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e15c6-116">Azure PowerShell</span></span>

<span data-ttu-id="e15c6-117">Använder du följande kommando för att ta bort klustret från en PowerShell-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="e15c6-117">From a PowerShell prompt, use the following command to delete the cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

<span data-ttu-id="e15c6-118">Ersätt **KLUSTERNAMN** med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e15c6-118">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

## <a name="azure-cli-10"></a><span data-ttu-id="e15c6-119">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e15c6-119">Azure CLI 1.0</span></span>

<span data-ttu-id="e15c6-120">Använd följande från en kommandotolk för att ta bort klustret:</span><span class="sxs-lookup"><span data-stu-id="e15c6-120">From a prompt, use the following to delete the cluster:</span></span>

    azure hdinsight cluster delete CLUSTERNAME

<span data-ttu-id="e15c6-121">Ersätt **KLUSTERNAMN** med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e15c6-121">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="e15c6-122">Azure CLI 2.0 stöder inte ta bort HDInsight-kluster just nu (31 juli 2017).</span><span class="sxs-lookup"><span data-stu-id="e15c6-122">Azure CLI 2.0 does not support deleting HDInsight clusters at this time (July 31, 2017).</span></span>