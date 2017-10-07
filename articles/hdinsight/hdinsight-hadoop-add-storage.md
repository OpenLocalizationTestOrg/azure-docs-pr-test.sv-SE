---
title: aaaAdd ytterligare Azure storage-konton tooHDInsight | Microsoft Docs
description: "Lär dig hur tooadd ytterligare Azure storage-konton tooan befintligt HDInsight-kluster."
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
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: ce5acfa4b61bf7e83b1fb374d64a1eaa3182fbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-additional-storage-accounts-toohdinsight"></a>Lägga till ytterligare lagringsutrymme konton tooHDInsight

Lär dig hur toouse skript åtgärder tooadd ytterligare Azure storage-konton tooHDInsight. hello stegen i det här dokumentet lägga till ett storage-konto tooan befintliga Linux-baserat HDInsight-kluster.

> [!IMPORTANT]
> hello informationen i det här dokumentet handlar om att lägga till ytterligare lagringsutrymme tooa klustret när den har skapats. Mer information om att lägga till lagringskonton när klustret skapas finns [ställa in kluster i HDInsight Hadoop, Spark, Kafka och mycket mer](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Hur det fungerar

Det här skriptet tar hello följande parametrar:

* __Azure lagringskontonamnet__: hello namnet på hello storage-konto tooadd toohello HDInsight-kluster. När du har kört skriptet hello kan HDInsight läsa och skriva data lagras i det här lagringskontot.

* __Azure lagringskontonyckel__: en nyckel som beviljar åtkomst toohello storage-konto.

* __-p__ (valfritt): Om du hello nyckeln krypteras inte och lagras i hello core-site.xml filen som oformaterad text.

Under bearbetning av utför hello skript hello följande åtgärder:

* Om hello storage-konto finns redan i hello core-site.xml konfigurationen för hello kluster, hello skriptet avslutas och inga ytterligare åtgärder utförs.

* Verifierar att hello lagringskontot finns och kan nås med hello nyckel.

* Krypterar hello nyckeln med hjälp av autentiseringsuppgifter för hello-klustret.

* Lägger till hello konto toohello core-site.xml lagringsfilen.

* Stoppar och startar om hello Oozie, YARN, MapReduce2 och HDFS-tjänster. Stoppa och starta dessa tjänster kan de toouse hello nytt lagringskonto.

> [!WARNING]
> Med hjälp av ett lagringskonto i en annan plats än hello HDInsight-kluster stöds inte.

## <a name="hello-script"></a>hello-skript

__Script plats__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Krav för__:

* hello skriptet måste tillämpas på hello __huvudnoder__.

## <a name="toouse-hello-script"></a>toouse hello skript

Det här skriptet kan användas från hello Azure-portalen, Azure PowerShell eller hello Azure CLI 1.0. Mer information finns i hello [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentet.

> [!IMPORTANT]
> Använd följande information tooapply hello skriptet när du använder hello stegen i hello anpassning dokument:
>
> * Ersätt alla exempel skriptåtgärd URI med hello URI för det här skriptet (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).
> * Ersätt alla exempel parametrar med hello Azure storage-kontonamnet och nyckeln hello konto toobe tillagda toohello av klustret för lagring. Om du använder hello Azure-portalen, måste parametrarna avgränsas med ett blanksteg.
> * Du behöver inte toomark skriptet som __bevarade__, enligt den direkt uppdaterar hello Ambari konfigurationen för hello kluster.

## <a name="known-issues"></a>Kända problem

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Storage-konton som inte visas i Azure-portalen eller verktyg

När Visa hello HDInsight-klustret i hello Azure-portalen, välja hello __Lagringskonton__ posten under __egenskaper__ storage-konton som lagts till via den här skriptåtgärden visas inte. Azure PowerShell och Azure CLI visas inte hello ytterligare storage-konto.

hello storage-informationen visas inte eftersom hello skript ändras endast hello core-site.xml konfigurationen för hello kluster. Den här informationen används inte vid hämtning av hello klustret information med hjälp av Azure management API: er.

information om lagringskontot tooview lägga toohello kluster med hjälp av det här skriptet kan använda hello Ambari REST API. Använd följande kommandon tooretrieve hello informationen för klustret:

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter hello cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> Ange `$clusterName` toohello namnet på hello HDInsight-kluster. Ange `$storageAccountName` toohello namnet på hello storage-konto. När du uppmanas ange hello klusterinloggning (admin) och lösenord.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> Ange `$PASSWORD` toohello klustret inloggning (admin) lösenord. Ange `$CLUSTERNAME` toohello namnet på hello HDInsight-kluster. Ange `$STORAGEACCOUNTNAME` toohello namnet på hello storage-konto.
>
> Det här exemplet används [curl (http://curl.haxx.se/)](http://curl.haxx.se/) och [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve och parsa JSON-data.

När det här kommandot ersätter __KLUSTERNAMN__ med hello namnet hello HDInsight-kluster. Ersätt __lösenord__ med hello HTTP inloggningslösenordet för hello-kluster. Ersätt __STORAGEACCOUNT__ med hello namn som lagts till med skriptåtgärder hello storage-konto. Informationen som returneras från det här kommandot visas liknande toohello följande text:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Den här texten är ett exempel på en krypterad nyckel som används för tooaccess hello storage-konto.

### <a name="unable-tooaccess-storage-after-changing-key"></a>Det går inte tooaccess lagring när du har ändrat nyckel

Om du ändrar hello nyckeln för ett lagringskonto HDInsight inte längre komma åt hello storage-konto. HDInsight använder en cachelagrad kopia av nyckeln i hello core-site.xml för hello-kluster. Den här kopian måste vara uppdaterade toomatch hello ny nyckel.

Hello skriptet igen körs __inte__ uppdatera hello nyckeln som hello skriptet kontrollerar toosee om det finns redan en post för hello storage-konto. Om det finns redan en post, den inte göra några ändringar.

toowork problemet måste du ta bort hello befintlig post för hello storage-konto. Använd följande steg tooremove hello befintlig post hello:

1. Öppna hello Ambari-Webbgränssnittet för ditt HDInsight-kluster i en webbläsare. hello URI är https://CLUSTERNAME.azurehdinsight.net. Ersätt __KLUSTERNAMN__ med hello namnet på klustret.

    När du uppmanas ange hello HTTP-inloggning och lösenord för klustret.

2. Hello listan över tjänster hello till vänster i hello sidan Välj __HDFS__. Välj hello __konfigurationerna__ fliken i hello center hello-sidan.

3. I hello __Filter...__  , ange ett värde för __fs.azure.account__. Detta returnerar poster för alla ytterligare lagringskonton som har lagts till toohello klustret. Det finns två typer av uppgifter. __keyprovider__ och __nyckeln__. Innehåller båda hello namnet på hello lagringskonto som en del av hello nyckelnamn.

    hello följande är Exempelposter för ett lagringskonto med namnet __mystorage__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. När du har identifierat hello nycklar för hello storage-konto behöver du tooremove använda hello red '-' toohello ikonen till höger om hello post toodelete den. Använd hello __spara__ knappen toosave ändringarna.

5. När ändringarna har sparats, använda hello skript åtgärd tooadd hello storage-konto och nya nyckelvärdet toohello klustret.

### <a name="poor-performance"></a>Dåliga prestanda

Om hello storage-konto finns i en annan region än hello HDInsight-kluster, kan det uppstå sämre prestanda. Åtkomst till data i en annan region skickar trafik utanför hello regionala Azure-datacenter och tvärs över hello offentliga internet, som kan introducera svarstid.

> [!WARNING]
> Med hjälp av ett lagringskonto i en annan region än hello HDInsight-kluster stöds inte.

### <a name="additional-charges"></a>Ytterligare kostnader

Om hello storage-konto har en annan region än hello HDInsight-kluster, kan du eventuellt se ytterligare utgång avgifter på ditt Azure fakturering. En utgående kostnad tillämpas när data lämnar ett regionala datacenter. Det här tillägget används även om hello trafik är avsedda för en annan Azure-datacenter i en annan region.

> [!WARNING]
> Med hjälp av ett lagringskonto i en annan region än hello HDInsight-kluster stöds inte.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur tooadd ytterligare lagringskonton tooan befintligt HDInsight-kluster. Mer information om skriptåtgärder finns [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)
