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
# <a name="upgrade-hdinsight-cluster-tooa-newer-version"></a>HDInsight-kluster tooa nyare uppgraderingsversion
tootake nytta av Hej senaste HDInsight-funktionerna, rekommenderar vi att uppgraderade toolatest version på HDInsight-kluster. Följ hello nedan riktlinjer tooupgrade HDInsight-kluster-versioner.

> [!NOTE]
> HDInsight-kluster version 3.2 eller 3.3 snart datumet för tillbakadragandet. Information om HDInsight version som stöds finns [HDInsight komponenten versioner](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Uppgraderingsuppgifter
hello arbetsflöde tooupgrade HDInsight-kluster är som följer.

![Uppgradera arbetsflödesdiagram](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Läs avsnitten i det här dokumentet toounderstand ändringar som kan krävas när du uppgraderar ditt HDInsight-kluster.
2. Skapa ett kluster som en test/kvalitet försäkran miljö. Mer information om hur du skapar ett kluster finns [Lär dig hur toocreate Linux-baserade HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)
3. Kopiera befintliga jobb, datakällor, och egenskaperna toohello nya miljön. Se [kopieringsdata tooTest miljö](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) för mer information.
4. Verifieringen testning toomake till att dina jobb fungerar som förväntat på hello nya klustret.


När du har kontrollerat att allt fungerar som förväntat, Schemalägg driftstopp för hello migrering. Under den här driftstörningen hello gör du följande åtgärder:

1.  Säkerhetskopiera alla tillfälligt data som lagras lokalt på hello klusternoder. Till exempel om du har data som lagras direkt på en huvudnod.
2.  Ta bort hello befintligt kluster.
3.  Skapa ett kluster i hello samma virtuella nätverk undernät med senaste (eller stöds) HDI version med hello samma standard datalager som hello tidigare kluster används. Detta gör att hello nya kluster toocontinue arbeta mot ditt befintliga produktionsdata.
4.  Importera alla tillfälligt säkerhetskopierade data.
5.  Starta jobb/fortsätta att bearbeta med hello nytt kluster.

## <a name="next-steps"></a>Nästa steg
* [Lär dig hur toocreate Linux-baserade HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)
* [Ansluta tooHDInsight via SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Hantera en Linux-baserade kluster med Ambari](hdinsight-hadoop-manage-ambari.md)

