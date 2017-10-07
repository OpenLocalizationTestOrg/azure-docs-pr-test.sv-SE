---
title: aaaScript utveckling med Linux-baserade HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse Bash skript toocustomize Linux-baserade HDInsight-kluster. hello skript åtgärd funktion i HDInsight kan du toorun skript under eller efter att klustret har skapats. Skript kan använda toochange inställningar för klustrets eller installera ytterligare programvara."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 1f504b00365df5f4cfb3ae19ad55ff7630342650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="script-action-development-with-hdinsight"></a>Skriptutveckling med HDInsight

Lär dig hur toocustomize ditt HDInsight-kluster med Bash skript. Skriptåtgärder är ett sätt toocustomize HDInsight under eller efter att klustret har skapats.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-are-script-actions"></a>Vad är skriptåtgärder

Skriptåtgärder Bash-skript som Azure körs på hello ändringar i klusterkonfigurationen noder toomake eller installera programvara. En skriptåtgärd körs som rot och ger fullständig åtkomst rättigheter toohello klusternoder.

Skriptåtgärder kan tillämpas via hello följande metoder:

| Använd den här metoden tooapply skript... | Skapa kluster under... | På ett kluster som körs... |
| --- |:---:|:---:|
| Azure Portal |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure CLI |&nbsp; |✓ |
| HDInsight .NET SDK |✓ |✓ |
| Azure Resource Manager-mall |✓ |&nbsp; |

Mer information om hur du använder dessa metoder tooapply skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Metodtips för skriptutveckling av

När du utvecklar ett anpassat skript för ett HDInsight-kluster finns flera bästa praxis tookeep i åtanke:

* [Målversionen hello Hadoop](#bPS1)
* [Mål hello OS-Version](#bps10)
* [Ange stabil länkar tooscript resurser](#bPS2)
* [Använda fördefinierade kompilerade resurser](#bPS4)
* [Se till att hello kluster anpassning skriptet idempotent](#bPS3)
* [Garantera hög tillgänglighet för hello kluster-arkitektur](#bPS5)
* [Konfigurera hello anpassade komponenter toouse Azure Blob storage](#bPS6)
* [Skriva information tooSTDOUT och STDERR](#bPS7)
* [Spara filer i ASCII-format med LF radbrytningar](#bps8)
* [Använd försök logik toorecover från tillfälliga fel](#bps9)

> [!IMPORTANT]
> Skriptåtgärder måste slutföras inom 60 minuter eller hello misslyckas. Under noden etableringen körs hello skriptet samtidigt med andra processer för installation och konfiguration. Konkurrens om resurser, till exempel CPU-tid eller nätverket bandbredd kan orsaka hello skriptet tootake längre toofinish än i din utvecklingsmiljö.

### <a name="bPS1"></a>Målversionen hello Hadoop

Olika versioner av HDInsight har olika versioner av Hadoop-tjänster och komponenter installeras. Om skriptet förväntar en viss version av en tjänst eller en komponent, bör du bara använda hello skript med hello version av HDInsight som innehåller hello nödvändiga komponenter. Du hittar information om komponenten-versioner som ingår i HDInsight med hjälp av hello [HDInsight component-versioning](hdinsight-component-versioning.md) dokumentet.

### <a name="bps10"></a>Målversionen hello OS

Linux-baserat HDInsight baseras på hello Ubuntu Linux-distribution. Olika versioner av HDInsight förlitar sig på olika versioner av Ubuntu som kan ändra hur skriptet fungerar. Till exempel HDInsight 3,4 och tidigare baseras på Ubuntu-versioner som använder Upstart. Version 3.5 är baserad på Ubuntu 16.04 som använder Systemd. Systemd och Upstart förlitar sig på olika kommandon så att skriptet ska skrivas toowork med båda.

En annan viktig skillnad mellan HDInsight 3.4 och 3.5 är att `JAVA_HOME` pekar nu tooJava 8.

Du kan kontrollera hello OS-version med hjälp av `lsb_release`. hello följande kod visar hur toodetermine om hello skript körs på Ubuntu 14 eller 16:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Du kan hitta hello fullständig skript som innehåller dessa kodavsnitt på https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Hello version av Ubuntu som används av HDInsight finns hello [HDInsight komponentversionen](hdinsight-component-versioning.md) dokumentet.

toounderstand hello skillnader mellan Systemd och Upstart, se [Systemd för Upstart användare](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Ange stabil länkar tooscript resurser

hello måste skript och associerade resurser vara tillgängliga i hela hello livstid hello-klustret. Dessa resurser krävs om nya noder läggs toohello kluster under skalning åtgärder.

hello bästa praxis är toodownload och arkivera allt innehåll i ett Azure Storage-konto på din prenumeration.

> [!IMPORTANT]
> hello storage-konto som används måste vara hello standardkontot för lagring för hello kluster eller en offentlig, skrivskyddad behållare för andra storage-konto.

Till exempel hello-exempel som tillhandahålls av Microsoft lagras i hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage-konto. Detta är en offentlig, skrivskyddad behållare som underhålls av hello HDInsight-teamet.

### <a name="bPS4"></a>Använda fördefinierade kompilerade resurser

tooreduce hello tid det tar toorun hello skript, undvika åtgärder som sammanställer resurser från källkoden. Till exempel före kompilera resurser och lagrar dem i en blob med Azure Storage-konto i hello samma datacenter som HDInsight.

### <a name="bPS3"></a>Se till att hello kluster anpassning skriptet idempotent

Skript måste vara idempotent. Om hello skriptet körs flera gånger, som det ska returnera hello klustret toohello samma tillstånd varje gång.

Till exempel ett skript som ändrar konfigurationsfiler bör inte lägga till dubblettposter om körde flera gånger.

### <a name="bPS5"></a>Garantera hög tillgänglighet för hello kluster-arkitektur

Linux-baserade HDInsight-kluster tillhandahåller två huvudnoderna som är aktiva i hello kluster och skriptåtgärder köras på båda noderna. Om hello komponenter du installerar förväntas bara en huvudnod kan du inte installera hello-komponenter på båda huvudnoderna.

> [!IMPORTANT]
> Tjänster som tillhandahålls som en del av HDInsight är utformad toofail över mellan hello två huvudnoderna efter behov. Den här funktionen är inte utökat toocustom komponenter som installeras via skriptåtgärder. Om du behöver hög tillgänglighet för anpassade komponenter måste du implementera en egen mekanism för växling vid fel.

### <a name="bPS6"></a>Konfigurera hello anpassade komponenter toouse Azure Blob storage

Komponenter som installeras på hello klustret kan ha en standardkonfiguration som använder Hadoop Distributed File System (HDFS) lagring. HDInsight använder antingen Azure Storage eller Azure Data Lake Store som hello standardlagring. Båda ger ett kompatibelt HDFS-filsystem som kvarstår data även om hello kluster har tagits bort. Du kan behöva tooconfigure komponenter du installerar toouse WASB eller ADL i stället för HDFS.

De flesta åtgärder behöver du inte toospecify hello-filsystemet. Till exempel kopierar hello följande hello giraph examples.jar fil från hello lokala system toocluster fillagring:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

I det här exemplet hello `hdfs` kommando använder transparent hello standard klusterlagringen. För vissa åtgärder, kanske du måste toospecify hello URI. Till exempel `adl:///example/jars` för Data Lake Store eller `wasb:///example/jars` för Azure Storage.

### <a name="bPS7"></a>Skriva information tooSTDOUT och STDERR

HDInsight loggar utdata från skriptet som är skrivna tooSTDOUT och STDERR. Du kan visa den här informationen med hjälp av hello Ambari-webbgränssnittet.

> [!NOTE]
> Ambari är endast tillgänglig om hello klustret har skapats. Om du använder en skriptåtgärd under klustret har skapats och skapa misslyckas, finns i avsnittet om felsökning hello [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) för andra sätt att komma åt loggade informationen.

De flesta verktyg och Installationspaketen skriva redan information tooSTDOUT och STDERR, men du kanske vill tooadd ytterligare loggning. toosend text tooSTDOUT, Använd `echo`. Exempel:

```bash
echo "Getting ready tooinstall Foo"
```

Som standard `echo` skickar hello sträng tooSTDOUT. toodirect den tooSTDERR, lägga till `>&2` innan `echo`. Exempel:

```bash
>&2 echo "An error occurred installing Foo"
```

Detta omdirigerar information skrivs tooSTDOUT tooSTDERR (2) i stället. Mer information om i/o-omdirigering finns [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Mer information om hur du visar information som loggas av skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a>Spara filer i ASCII-format med LF radbrytningar

Bash-skript ska lagras som ASCII-format med rader som avslutas av LF. Filer som lagras som UTF-8 eller använda CRLF som hello rad avslutas kan misslyckas med hello följande fel:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a>Använd försök logik toorecover från tillfälliga fel

Vid hämtning av filer, installera paket med lgh get eller andra åtgärder som överför data via Hej internet, hello åtgärden misslyckas på grund av tootransient nätverksfel. Exempelvis kanske hello fjärresursen du kommunicerar med pågående hello misslyckas över tooa säkerhetskopiering nod.

toomake din skriptfel flexibel tootransient, du kan implementera logik för omprövning. hello följande funktion visar hur tooimplement försök logik. Den försöker hello åtgärden tre gånger innan åtgärden misslyckas.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

hello följande exempel visar hur toouse den här funktionen.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Hjälpmetoder för anpassade skript

Skriptet åtgärd helper metoder är verktyg som du kan använda vid skrivning till anpassade skript. Metoderna finns i den[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) skript. Använd följande toodownload hello och använda dem som en del av skriptet:

```bash
# Import hello helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

hello efter hjälpprogram som är tillgängliga för användning i skriptet:

| Helper användning | Beskrivning |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Laddar ned en fil från hello URI toohello angivna sökvägen till källfilen. Som standard den inte över en befintlig fil. |
| `untar_file TARFILE DESTDIR` |Extraherar tar-filen (med `-xf`) toohello målkatalogen. |
| `test_is_headnode` |Om kördes på en klustrets huvudnod, returnerar 1. annars 0. |
| `test_is_datanode` |Om hello aktuella noden är en datanod (worker), returnera 1; annars 0. |
| `test_is_first_datanode` |Om hello aktuella noden hello första data (worker) nod (namngivna workernode0) returnera 1; annars 0. |
| `get_headnodes` |Returnera hello fullständigt kvalificerade domännamnet för hello headnodes i hello kluster. Namn är kommaavgränsade. En tom sträng returneras vid fel. |
| `get_primary_headnode` |Hämtar hello fullständigt kvalificerade domännamnet för hello primära headnode. En tom sträng returneras vid fel. |
| `get_secondary_headnode` |Hämtar hello fullständigt kvalificerade domännamnet för hello sekundära headnode. En tom sträng returneras vid fel. |
| `get_primary_headnode_number` |Hämtar hello numeriskt suffix hello primära headnode. En tom sträng returneras vid fel. |
| `get_secondary_headnode_number` |Hämtar hello numeriskt suffix hello sekundära headnode. En tom sträng returneras vid fel. |

## <a name="commonusage"></a>Vanliga användningsmönster

Det här avsnittet ger vägledning om att implementera några hello vanliga användningsmönster som som kan uppstå vid skrivning till ett eget skript.

### <a name="passing-parameters-tooa-script"></a>Skicka parametrar tooa skript

I vissa fall, kan skriptet kräver parametrar. Du kan exempelvis behöva hello administratörslösenord för hello klustret när du använder hello Ambari REST API.

Parametrar som skickas toohello skript kallas *positionsparametrarna*, och tilldelas för`$1` för hello första parametern, `$2` för hello andra och så på. `$0`innehåller hello namnet på själva hello-skriptet.

Argumenten toohello skript som parametrar ska omges av enkla citattecken ('). På så sätt att hello skickades värde behandlas som en literal.

### <a name="setting-environment-variables"></a>Ställa in miljövariabler

Ange en miljövariabel utförs av hello följande instruktion:

    VARIABLENAME=value

Där är VARIABELNAMN hello namnet på hello variabel. tooaccess hello variabel, Använd `$VARIABLENAME`. Till exempel tooassign ett värde som tillhandahålls av namngivna parametrar som en miljövariabel med namnet lösenord, använder du följande instruktion hello:

    PASSWORD=$1

Efterföljande åtkomst toohello information kan sedan använda `$PASSWORD`.

Miljövariabler som anges i hello skript finns bara i hello omfattning hello skript. I vissa fall kan behöva du tooadd systemomfattande miljövariabler som finns kvar efter hello skriptet har slutförts. tooadd systemomfattande miljövariabler, Lägg till hello variabel för`/etc/environment`. Till exempel hello följande uttryck lägger till `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a>Åtkomst till toolocations där hello anpassade skript lagras

Skript som används toocustomize ett kluster måste toobe lagras i något av följande platser hello:

* En __Azure Storage-konto__ som är associerad med hello-kluster.

* En __ytterligare lagringskonto__ som associerats med hello kluster.

* En __offentligt läsbar URI__. Till exempel en URL toodata lagras på OneDrive, Dropbox eller andra filer som är värd för tjänsten.

* En __Azure Data Lake Store-konto__ som är associerad med hello HDInsight-kluster. Mer information om hur du använder Azure Data Lake Store med HDInsight finns [skapar ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > hello service principal HDInsight använder tooaccess Data Lake Store måste ha läsbehörighet toohello skript.

Resurser som används av hello skriptet måste också vara offentligt tillgängliga.

Lagra hello filer i en Azure Storage-konto eller ett Azure Data Lake Store ger snabb åtkomst som både i hello Azure-nätverk.

> [!NOTE]
> hello URI-format som används för tooreference hello skript skiljer sig beroende på hello tjänsten används. Storage-konton som är associerade med hello HDInsight-kluster, använda `wasb://` eller `wasbs://`. Offentligt läsbar URI: er använda `http://` eller `https://`. Data Lake Store använder `adl://`.

### <a name="checking-hello-operating-system-version"></a>Kontrollerar hello operativsystemets version

Olika versioner av HDInsight är beroende av specifika versioner av Ubuntu. Det kan finnas skillnader mellan OS-versioner måste du kontrollera i skriptet. Du kan till exempel måste tooinstall en binärfil som är bundet toohello version av Ubuntu.

toocheck hello OS-version, Använd `lsb_release`. Hello följande skript visar exempelvis hur tooreference specifika tar filen beroende på hello OS-version:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Checklista för distribution av en skriptåtgärd

Här är hello steg som vi har tagit när du förbereder toodeploy dessa skript:

* Placera hello-filer som innehåller hello anpassade skript på en plats som kan nås av hello klusternoder under distributionen. Till exempel hello standardlagring för hello-kluster. Filer kan också lagras i offentligt läsbar värdtjänster.
* Kontrollera att skriptet hello är impotent. På så sätt kan hello skriptet toobe köras flera gånger på hello samma nod.
* Använd en tillfällig katalog /tmp tookeep hello nedladdade filer som används av hello skript och sedan rensa dem efter skript har körts.
* Om inställningar för OS-nivå eller Hadoop service configuration-filer har ändrats kanske toorestart HDInsight tjänster.

## <a name="runScriptAction"></a>Hur toorun en skriptåtgärd

Du kan använda skriptet åtgärder toocustomize HDInsight-kluster med hello följande metoder:

* Azure Portal
* Azure PowerShell
* Azure Resource Manager-mallar
* Hej HDInsight .NET SDK.

Mer information om hur du använder varje metod finns [hur toouse skript åtgärden](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Anpassade skriptexempel

Microsoft tillhandahåller exempelskript tooinstall komponenter på ett HDInsight-kluster. Se följande länkar för flera exempel skriptåtgärder hello.

* [Installera och använda Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md)
* [Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md)
* [Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md)
* [Installera eller uppgradera Mono på HDInsight-kluster](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a>Felsökning

hello följande är fel som kan uppstå när du använder skript som du har utvecklat:

**Fel**: `$'\r': command not found`. Ibland följt av `syntax error: unexpected end of file`.

*Orsak*: det här felet uppstår om hello rader i ett skript som avslutas med CRLF. UNIX-system förväntar sig endast LF som hello rad avslutas.

Det här problemet inträffar oftast när hello skript har skapats på en Windows-miljö CRLF är en gemensam rader som slutar med för många textredigerare på Windows.

*Lösning*: om det är ett alternativ i en textredigerare, Välj Unix-format eller LF för hello rad avslutas. Du kan även använda följande kommandon på en Unix system toochange hello CRLF tooan LF hello:

> [!NOTE]
> hello är följande kommandon ungefär att de ska ändra hello CRLF rad ändelser tooLF. Välj en baserat på hello-verktyg som finns på datorn.

| Kommando | Anteckningar |
| --- | --- |
| `unix2dos -b INFILE` |hello originalfilen säkerhetskopieras med en. BAK-tillägg |
| `tr -d '\r' < INFILE > OUTFILE` |UTFIL innehåller en version med endast LF ändelser |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Ändrar hello-filen direkt |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |UTFIL innehåller en version med endast LF ändelser. |

**Fel**: `line 1: #!/usr/bin/env: No such file or directory`.

*Orsak*: det här felet uppstår när hello skript har sparats som UTF-8 en Byte ordning markering (BOM).

*Lösning*: spara hello-fil som ASCII eller som UTF-8 utan en struktur. Du kan även använda följande kommando på en Linux eller Unix system-toocreate en fil utan hello BOM hello:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Ersätt `INFILE` med hello-fil som innehåller hello BOM. `OUTFILE`måste vara ett nytt filnamn som innehåller hello skript utan hello BOM.

## <a name="seeAlso"></a>Nästa steg

* Lär dig hur för[anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)
* Använd hello [HDInsight .NET SDK referens](https://msdn.microsoft.com/library/mt271028.aspx) toolearn mer om hur du skapar .NET-program som hanterar HDInsight
* Använd hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn hur toouse REST tooperform hanteringsåtgärder på HDInsight-kluster.
