---
title: "aaaConfigure OS uppdatering schema för Linux-baserat HDInsight - kluster i Azure | Microsoft Docs"
description: "Lär dig hur tooconfigure OS uppdatering schema för Linux-baserat HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: bhanupr
ms.openlocfilehash: 1598d64e594d7e8a68573fc63dd86051a5a9d025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="os-patching-for-hdinsight"></a>OS-korrigering för HDInsight 
Som en hanterad tjänst i Hadoop hand HDInsight tar om korrigering hello OS av hello underliggande virtuella datorer som används av HDInsight-kluster. Från och med 1 augusti 2016 har vi ändrat hello gäst OS uppdatering princip för Linux-baserade HDInsight-kluster (version 3.4 eller högre). hello målet hello nya principen är toosignificantly minska hello antalet omstarter på grund av toopatching. hello ny princip kommer att fortsätta toopatch virtuella maskiner (VMs) på Linux-kluster varje måndag eller torsdag som börjar vid 12: 00 UTC successiva överskrids över noderna i alla kluster. Alla angivna VM startas dock endast en gång var 30 dagar på grund av tooguest OS-korrigering. Dessutom sker hello första omstarten för ett nyskapat kluster inte tidigare än 30 dagar från hello skapandedatum för klustret. Korrigeringsfiler börjar gälla när hello virtuella datorer startas om.

## <a name="how-tooconfigure-hello-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Hur tooconfigure hello OS uppdatering schema för Linux-baserade HDInsight-kluster
hello virtuella datorer i ett HDInsight-kluster måste toobe startas om ibland så att viktiga säkerhetsuppdateringar kan installeras. Från och med 1 augusti 2016 kan nya Linux-baserat HDInsight-kluster (version 3.4 eller nyare) startas om med hello enligt schema:

1. En virtuell dator i klustret hello kan endast startas korrigeringsfiler som mest en gång i en 30-dagarsperiod.
2. hello omstart sker början från 12: 00 UTC.
3. hello omstart processen sprids över virtuella datorer i hello kluster så hello klustret fortfarande är tillgänglig under hello omstart.
4. hello första omstarten för ett nyskapat kluster sker inte tidigare än 30 dagar efter skapandedatum för hello-klustret.

Med hello skriptåtgärd som beskrivs i den här artikeln kan ändra du hello OS uppdatering schemat på följande sätt:
1. Aktivera eller inaktivera automatiska omstarter
2. Ange hello frekvensen av startas om (dagar mellan olika omstarter)
3. Ange hello veckodag hello när en omstart sker

> [!NOTE]
> Den här skriptåtgärden fungerar bara med Linux-baserat HDInsight-kluster som skapas efter den 1 augusti 2016. Korrigeringsfiler börjar gälla när virtuella datorer startas om. 
>

## <a name="how-toouse-hello-script"></a>Hur toouse hello skript 

När det här kräver hello följande information:
1. Hej skriptets placering: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight använder den här URI toofind och kör hello skriptet på alla hello virtuella datorer i hello kluster.
  
2. Hej klustret nodtyper som hello skript används: headnode, workernode, zookeeper. Det här skriptet måste vara tillämpade tooall nodtyper i hello kluster. Om den inte används tooa nodtyp kan hello virtuella fortsätter datorer för att nodtypen toouse hello tidigare uppdatering schemat.


3.  Parameter: Det här skriptet tar tre numeriska parametrar:

    | Parameter | Definition |
    | --- | --- |
    | Aktivera/inaktivera automatiska omstarter |0 eller 1. Värdet 0 inaktiverar automatiska omstarter när 1 aktiverar automatiska omstarter. |
    | frekvens |7 too90 (sammanlagt). hello antal dagar toowait före omstart hello virtuella datorer för korrigeringsprogram som kräver en omstart. |
    | Dag i veckan |1 too7 (sammanlagt). Värdet 1 anger hello omstart sker på en måndag och 7 visar en Sunday.For exempelvis med parametrar 1 60 2 leder till att automatiskt startar om var 60: e dag (mest) tisdagen. |
    | Beständiga |När du använder ett befintligt kluster för skriptet åtgärd tooan, kan du markera hello skript som bestående. Beständiga skript används när nya workernodes läggs toohello klustret via skalning åtgärder. |

> [!NOTE]
> Du måste markera det här skriptet som beständiga när du använder tooan befintligt kluster. Annars använder alla nya noder som skapats via skalning operations hello standard korrigering schema.
Om du tillämpar hello skriptet som en del av hello klusterskapandeprocessen kan sparas det automatiskt.
>

## <a name="next-steps"></a>Nästa steg

Mer specifika anvisningar för med hello skriptåtgärder finns i följande avsnitt i hello hello [anpassa Linuz-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md):

* [Använd en skriptåtgärd när klustret skapas](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Använda ett skript åtgärd tooa kör kluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
