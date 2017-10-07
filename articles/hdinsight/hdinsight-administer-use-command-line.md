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
# <a name="manage-hadoop-clusters-in-hdinsight-using-hello-azure-cli"></a>Hantera Hadoop-kluster i HDInsight med hello Azure CLI
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Lär dig hur toouse hello [Azure-kommandoradsgränssnittet](../cli-install-nodejs.md) toomanage Hadoop-kluster i Azure HDInsight. hello Azure CLI är implementerat i Node.js. Det kan användas på alla plattformar som har stöd för Node.js, inklusive Windows, Mac- och Linux.

Den här artikeln täcker endast hello Azure CLI med HDInsight. En allmän vägledning om hur toouse Azure CLI, se [installera och konfigurera Azure CLI][azure-command-line-tools].

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure CLI** -finns [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) information om installation och konfiguration.
* **Ansluta tooAzure**med hjälp av hello följande kommando:
  
        azure login
  
    Mer information om autentisering med ett arbets- eller skolkonto konto finns [ansluta tooan Azure-prenumeration från hello Azure CLI](../xplat-cli-connect.md).
* **Växla toohello Azure Resource Manager läge**med hjälp av hello följande kommando:
  
        azure config mode arm

tooget hjälp, använda hello **-h** växla.  Exempel:

    azure hdinsight cluster create -h

## <a name="create-clusters-with-hello-cli"></a>Skapa kluster med hello CLI
Se [Skapa kluster i HDInsight med hello Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Listan och visa information om kluster
Använd följande kommandon toolist hello och visa information om kluster:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![Kommandoradsverktyget vy av klustret lista][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Ta bort kluster
Använd följande kommando toodelete ett kluster hello:

    azure hdinsight cluster delete <Cluster Name>

Du kan också ta bort ett kluster genom att ta bort hello resursgruppen som innehåller hello klustret. Observera detta tar bort alla hello resurser i hello grupp, inklusive hello standardkontot för lagring.

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a>Skala kluster
toochange hello Hadoop klusterstorleken:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Aktivera/inaktivera HTTP-åtkomst för ett kluster
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Aktivera/inaktivera RDP-åtkomst för ett kluster
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur tooperform olika HDInsight-kluster administrativa uppgifter. toolearn se fler hello följande artiklar:

* [Administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal]
* [Administrera HDInsight med hjälp av Azure PowerShell][hdinsight-admin-powershell]
* [Kom igång med Azure HDInsight][hdinsight-get-started]
* [Hur toouse hello Azure CLI][azure-command-line-tools]

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
