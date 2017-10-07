---
title: "Skapa - Azure för aaaAdd Hive-bibliotek under HDInsight-kluster | Microsoft Docs"
description: "Lär dig hur tooadd Hive-bibliotek (jar-filer), tooan HDInsight-kluster när klustret skapas."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 2e028a07c3248205def0789af2c262a0774a8f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Lägg till anpassade Hive-bibliotek när du skapar ditt HDInsight-kluster

Om du har bibliotek som du använder ofta med Hive i HDInsight kan innehåller det här dokumentet information om hur du använder en skriptåtgärd toopre belastningen hello bibliotek när klustret skapas. Bibliotek som lagts till med hello stegen i det här dokumentet är globalt tillgänglig i Hive - det finns inget behov av toouse [JAR-Lägg till](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload dem.

## <a name="how-it-works"></a>Hur det fungerar

Du kan också ange en skriptåtgärd som kör ett skript på klusternoder hello medan de skapas när du skapar ett kluster. hello skript i det här dokumentet accepterar en enda parameter som är en WASB-plats som innehåller hello bibliotek (som lagras som jar-filer) toobe redan har lästs in.

När klustret skapas hello skript räknar hello filer, kopierar dem toohello `/usr/lib/customhivelibs/` katalog på head- och arbetsroller noder och lägger sedan till dem toohello `hive.aux.jars.path` egenskap i hello `core-site.xml` fil. På Linux-baserade kluster även uppdateras hello `hive-env.sh` fil med hello var hello-filer.

> [!NOTE]
> Med hjälp av hello skriptåtgärder i den här artikeln tillgängliggör hello bibliotek i hello följande scenarier:
>
> * **Linux-baserat HDInsight** – när du använder hello en Hive-klient **WebHCat**, och **HiveServer2**.
> * **Windows-baserade HDInsight** – när hello Hive-klienten och **WebHCat**.

## <a name="hello-script"></a>hello-skript

**Skriptets placering**

För **Linux-baserade kluster**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

För **Windows-baserade kluster**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

**Krav**

* hello skript måste vara tillämpade tooboth hello **huvudnoder** och **arbetarnoder**.

* Hej burkar gärna tooinstall måste lagras i Azure Blob Storage i en **enskild behållare**.

* hello storage-konto som innehåller hello bibliotek med jar-filer **måste** vara länkade toohello HDInsight-kluster när du skapar. Det måste antingen vara hello standardkontot för lagring eller ett konto som har lagts till via __valfri konfiguration__.

* Hej WASB sökväg toohello behållaren måste anges som en parameter toohello skriptåtgärder. Till exempel om hello burkar lagras i en behållare med namnet **libs** på ett lagringskonto med namnet **mystorage**, hello parametern skulle vara  **wasb://libs@mystorage.blob.core.windows.net/** .

  > [!NOTE]
  > Det här dokumentet förutsätts att du redan har skapar ett lagringskonto, blob-behållaren och överförda hello filer tooit.
  >
  > Om du inte har skapat ett lagringskonto kan du göra det via hello [Azure-portalen](https://portal.azure.com). Du kan sedan använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/) toocreate en behållare i hello-konto och ladda upp filer tooit.

## <a name="create-a-cluster-using-hello-script"></a>Skapa ett kluster med hello skript

> [!NOTE]
> hello följande skapa ett Linux-baserat HDInsight-kluster. toocreate ett Windows-baserade kluster, Välj **Windows** som hello kluster-OS när du skapar hello kluster och använda hello Windows (PowerShell) skript i stället för hello bash-skript.
>
> Du kan också använda Azure PowerShell eller hello HDInsight .NET SDK toocreate ett kluster med det här skriptet. Mer information om hur du använder metoderna finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

1. Börja etablera ett kluster med hjälp av hello stegen i [etablera HDInsight-kluster på Linux](hdinsight-hadoop-provision-linux-clusters.md), men inte slutföra etablering.

2. På hello **valfri konfiguration** bladet väljer **skriptåtgärder**, och ange hello följande information:

   * **NAMNET**: Ange ett eget namn för hello skriptåtgärder.

   * **SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: Markera det här alternativet.

   * **WORKER**: Markera det här alternativet.

   * **ZOOKEEPER**: lämnar fältet tomt.

   * **PARAMETRARNA**: Ange hello WASB adress toohello behållare och storage-konto som innehåller hello burkar. Till exempel  **wasb://libs@mystorage.blob.core.windows.net/** .

3. Längst ned hello hello **skriptåtgärder**, använda hello **Välj** toosave hello konfiguration.

4. På hello **valfri konfiguration** bladet väljer **länkade Lagringskonton** och välj hello **lägga till en nyckel för säkerhetslagring** länk. Välj hello storage-konto som innehåller hello burkar och Använd sedan hello **Välj** knappar toosave inställningar och returnera hello **valfri konfiguration** bladet.

5. Använd hello **Välj** knappen längst ned hello hello **valfri konfiguration** bladet toosave hello ytterligare konfigurationsinformation.

6. Fortsätta etablering hello klustret enligt beskrivningen i [etablera HDInsight-kluster på Linux](hdinsight-hadoop-provision-linux-clusters.md).

När klustret skapas är klar är du kan toouse hello burkar lagts till via det här skriptet från Hive utan toouse hello `ADD JAR` instruktionen.

## <a name="next-steps"></a>Nästa steg

Mer information om hur du arbetar med Hive finns [använda Hive med HDInsight](hdinsight-use-hive.md)
