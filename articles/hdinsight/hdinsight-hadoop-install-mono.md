---
title: "aaaInstall eller uppdatera Mono på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse en viss version av Mono med HDInsight-kluster. Mono är används toorun .NET-program på Linux-baserade HDInsight-kluster."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: 1e8a8aaeff231c93a9e232379448517b326da965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a>Installera eller uppdatera Mono på HDInsight

Lär dig hur tooinstall en viss version av [Mono](https://www.mono-project.com) på HDInsight 3.4 eller högre.

Mono är installerad på HDInsight 3.4 och högre och har använt toorun .NET-program. Mer information om hello version Mono som ingår i varje version av HDInsight finns [HDInsight component-versioning](hdinsight-component-versioning.md). tooinstall en annan version på klustret, Använd hello skriptåtgärd i det här dokumentet. 

## <a name="how-it-works"></a>Hur det fungerar

Det här skriptet accepterar hello följande parameter:

* __Monoljud versionsnumret__: hello version av monoljud tooinstall. hello-version måste vara tillgänglig från [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

hello skriptet installerar hello följande monoljud paket:

* __slutföra Mono__

* __CA-certifikat-mono__

## <a name="hello-script"></a>hello-skript

__Script plats__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Krav för__:

* hello skriptet måste tillämpas på hello __huvudnoder__ och __arbetarnoder__.

## <a name="toouse-hello-script"></a>toouse hello skript

Mer information om hur toouse skriptet med HDInsight, se hello [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentet. Du kan använda skriptet hello via hello Azure-portalen, Azure PowerShell eller hello Azure CLI.

Följande hello skript åtgärd dokument, använda hello följande URI:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> När du konfigurerar HDInsight med det här skriptet, markera hello skriptet som __bevarade__. Den här inställningen kan HDInsight tooapply hello skriptet tooworker noder som har lagts till via skalning åtgärder.


## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur tooupgrade eller installera en viss version av Mono på HDInsight. Mer information om hur du använder .NET-program med Mono på HDInsight finns i hello följande dokument:

* [Använda .NET för strömning MapReduce på HDInsight](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Använda .NET med Hive och Pig i HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [Utveckla C#-lösningar med Storm på HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Migrera .NET lösningar tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Mer information om hur du använder skriptåtgärder finns [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)