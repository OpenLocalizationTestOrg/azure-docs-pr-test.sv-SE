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
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-hello-azure-cli"></a>Ta bort ett HDInsight-kluster med hjälp av din webbläsare, PowerShell eller hello Azure CLI

HDInsight-kluster faktureringen påbörjas så snart ett kluster skapas och stoppas när hello kluster har tagits bort. Fakturering är proportionerligt per minut, så du bör alltid ta bort klustret när den inte längre används. I detta dokument kan du lära dig hur toodelete ett kluster som använder hello Azure-portalen, Azure PowerShell och hello Azure CLI 1.0.

> [!IMPORTANT]
> Tar bort ett HDInsight-kluster tas inte bort hello Azure Storage-konton eller som är associerade med klustret hello Data Lake Store. Du kan återanvända data som lagras i dessa tjänster i hello framtida.

## <a name="azure-portal"></a>Azure Portal

1. Logga in toohello [Azure-portalen](https://portal.azure.com) och välj ditt HDInsight-kluster. Om ditt HDInsight-kluster inte är fäst toohello instrumentpanelen, kan du söka efter den efter namn med hello sökfältet.
   
    ![Portal-sökning](./media/hdinsight-delete-cluster/navbar.png)

2. När hello blad öppnas för hello kluster, Välj hello **ta bort** ikon. När du uppmanas, Välj **Ja** toodelete hello klustret.
   
    ![Ikonen Ta bort](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Använd hello efter kommandot toodelete hello kluster från en PowerShell-kommandotolk:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.

## <a name="azure-cli-10"></a>Azure CLI 1.0

Använd hello följande toodelete hello kluster från en kommandotolk:

    azure hdinsight cluster delete CLUSTERNAME

Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster.

> [!NOTE]
> Azure CLI 2.0 stöder inte ta bort HDInsight-kluster just nu (31 juli 2017).