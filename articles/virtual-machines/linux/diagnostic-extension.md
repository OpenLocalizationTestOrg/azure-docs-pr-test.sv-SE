---
title: "aaaAzure Compute - Linux diagnostiska tillägget | Microsoft Docs"
description: "Hur tooconfigure hello Azure Linux diagnostiska tillägg (LAD) toocollect mått och loggar händelser från virtuella Linux-datorer körs i Azure."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 2b27ec00a876ded359a75170b407e28c40d8445d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-linux-diagnostic-extension-toomonitor-metrics-and-logs"></a>Använd Linux diagnostiska tillägget toomonitor mått och loggar

Det här dokumentet beskriver version 3.0 och senare av hello Linux diagnostiska tillägg.

> [!IMPORTANT]
> Information om version 2.3 och äldre finns [dokumentet](./classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Introduktion

hello Linux diagnostiska tillägget hjälper användare Övervakare hello hälsa för en Linux-VM som körs på Microsoft Azure. Det har hello följande funktioner:

* Samlar in system prestandamått från hello VM och lagrar dem i en viss tabell i ett avsedda storage-konto.
* Hämtar händelser från syslog och lagrar dem i en viss tabell i hello avses storage-konto.
* Låter användare toocustomize hello data mått som samlas in och överföra.
* Kan användare toocustomize hello syslog verksamhet och allvarlighetsgrader för händelser som samlas in och överföra.
* Låter användare tooupload angivna filer tooa avsedda lagringstabellen.
* Stöder skicka mått och logga händelser tooarbitrary EventHub slutpunkter och JSON-formaterad blobbar i hello utsedda storage-konto.

Det här tillägget fungerar med båda modellerna i Azure-distribution.

## <a name="installing-hello-extension-in-your-vm"></a>Installera hello-tillägget på den virtuella datorn

Du kan aktivera det här tillägget med hjälp av hello Azure PowerShell-cmdlets, Azure CLI-skript eller mallar för Azure-distribution. Mer information finns i [tillägg funktioner](./extensions-features.md).

hello Azure-portalen kan inte använda tooenable eller konfigurera LAD 3.0. Istället installerar och konfigurerar version 2.3. Azure portal diagram och aviseringar kan du arbeta med data från båda versionerna av hello-tillägget.

Dessa instruktioner för installation och en [nedladdningsbara exempelkonfiguration](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) konfigurera LAD 3.0 till:

* avbilda och lagra hello samma mått som tillhandahålls av LAD 2.3;
* avbilda en användbar uppsättning filen system statistik, nya tooLAD 3.0;
* Avbilda hello syslog standardsamling aktiveras med LAD 2.3;
* Aktivera hello Azure portal upplevelsen för diagram och varningar på VM-mått.

hello nedladdningsbara konfigurationen är bara ett exempel. ändra den toosuit dina egna behov.

### <a name="prerequisites"></a>Krav

* **Azure Linux-agentens version 2.2.0 eller senare**. De flesta Azure VM Linux galleriavbildningar inkluderar version 2.2.7 eller senare. Kör `/usr/sbin/waagent -version` tooconfirm hello version är installerad på hello VM. Så om hello VM använder en äldre version av gästagenten för hello [instruktionerna](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate den.
* **Azure CLI**. [Ställ in hello Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) miljö på din dator.
* Hej wget-kommando om du inte redan har det: kör `sudo apt-get install wget`.
* En befintlig Azure-prenumeration och befintligt lagringsutrymme för ett konto i den toostore hello data.

### <a name="sample-installation"></a>Exempel installation

Fyll i hello rätt parametrar på hello först tre rader och sedan köra skriptet som rot:

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login tooAzure first before anything else
az login

# Select hello subscription containing hello storage account
az account set --subscription <your_azure_subscription_id>

# Download hello sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build hello VM resource ID. Replace storage account name and resource ID in hello public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build hello protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure tooinstall and enable hello extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

hello-URL för hello exempelkonfiguration och dess innehåll är ämne toochange. Hämta en kopia av hello portalinställningar JSON-fil och anpassa den efter dina behov. Alla mallar eller automatisering som du bör använda din egen kopia, i stället för att hämta URL: en varje gång.

### <a name="updating-hello-extension-settings"></a>Uppdaterar inställningar för hello-tillägg

När du har ändrat din skyddade eller inställningar för offentliga kan distribuera dem toohello VM genom att köra hello samma kommando. Om något ändras i inställningarna för hello skickas hello uppdaterade inställningar toohello tillägg. LAD läser hello-konfigurationen och startar om sig själv.

### <a name="migration-from-previous-versions-of-hello-extension"></a>Migrering från tidigare versioner av hello-tillägg

hello senaste versionen av tillägget hello **3.0**. **Eventuella gamla versioner (2.x) är föråldrade och kan vara opublicerad på eller efter den 31 juli 2018**.

> [!IMPORTANT]
> Det här tillägget introducerar senaste ändringar toohello konfigurationen av hello-tillägget. En ändring har gjorts tooimprove hello säkerheten för hello tillägget; Därför kan bakåtkompatibilitet kompatibilitet med 2.x inte upprätthållas. Dessutom skiljer hello tillägget Publisher för detta tillägg sig hello utgivaren för hello 2.x-versionerna.
>
> toomigrate från 2.x toothis ny version av hello tillägget måste du avinstallera hello gamla tillägget (under hello gamla utgivarens namn) och sedan installera version 3 för hello-tillägget.

Rekommendationer:

* Installera hello-tillägget med automatisk mindre versionsuppgradering aktiverad.
  * På klassiska distributionsmodellen VMs, anger du '3.*' hello versionen om du installerar tillägget hello via Azure XPLAT CLI eller Powershell.
  * Modellen virtuella datorer på Azure Resource Manager-distribution kan du ange ' ”autoUpgradeMinorVersion”: true' i hello mall för distribution.
* Använd en ny/annat lagringskonto för LAD 3.0. Det finns flera små inkonsekvenser mellan LAD 2.3 och LAD 3.0 som gör att dela ett program som krånglar konto:
  * LAD 3.0 lagrar syslog-händelser i en tabell med ett annat namn.
  * Hej counterSpecifier strängarna för `builtin` mått skiljer sig åt i LAD 3.0.

## <a name="protected-settings"></a>Skyddade inställningar

Denna uppsättning konfigurationsinformation innehåller känslig information som borde vara skyddad från den gemensamma vyn, till exempel lagring autentiseringsuppgifter. Dessa inställningar är överförda tooand lagras av hello tillägg i krypterad form.

```json
{
    "storageAccountName" : "hello storage account tooreceive data",
    "storageAccountEndPoint": "hello hostname suffix for hello cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

Namn | Värde
---- | -----
storageAccountName | hello namnet på hello lagringskonto där data skrivs av hello-tillägget.
storageAccountEndPoint | (valfritt) hello-slutpunkt som identifierar hello moln i vilka hello lagringskontot finns. Om den här inställningen saknas LAD standard toohello offentliga Azure-molnet, `https://core.windows.net`. toouse ett lagringskonto i Azure Tyskland eller Azure Government Azure Kina, ange värdet i enlighet med detta.
storageAccountSasToken | En [konto-SAS-token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) för Blob- och tjänster (`ss='bt'`), tillämpliga toocontainers och objekt (`srt='co'`), vilket ger lägga till, skapa, visa, uppdatera och skrivbehörighet (`sp='acluw'`). Gör *inte* inkluderar hello inledande frågetecken (?).
mdsdHttpProxy | (valfritt) HTTP-proxy information som behövs för tooenable hello tillägget tooconnect toohello angetts storage-konto och slutpunkt.
sinksConfig | (valfritt) Information om alternativa mål toowhich mått och händelser levereras. hello avsnitten som följer beskrivs hello närmare för varje mottagare för data som stöds av hello extension.

Du kan enkelt skapa hello krävs SAS-token via hello Azure-portalen.

1. Välj önskade hello tillägget toowrite hello Allmänt lagringskonto konto toowhich
1. Välj ”delad åtkomstsignatur” från hello inställningar tillhör hello vänstra menyn
1. Se hello relevanta avsnitt enligt beskrivningen ovan
1. Klicka hello ”generera SAS”.

![Bild](./media/diagnostic-extension/make_sas.png)

Kopiera hello genereras SAS i hello storageAccountSasToken område. ta bort hello inledande frågetecken (””?).

### <a name="sinksconfig"></a>sinksConfig

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

Det här valfria avsnittet definierar ytterligare mål toowhich hello tillägget skickar hello information som samlas in. Hej ”sink” matris innehåller ett objekt för varje ytterligare data sink. attributet ”typ” avgör hello hello andra attribut i hello-objektet.

Element | Värde
------- | -----
namn | En sträng används toorefer toothis sink någon annanstans i hello tilläggets konfiguration.
typ | hello typ av mottagare som definieras. Anger hello andra värden (eventuella) i instanser av den här typen.

Hello Linux diagnostiska tillägg version 3.0 stöder två typer av mottagare: EventHub och JsonBlob.

#### <a name="hello-eventhub-sink"></a>Hej EventHub mottagare

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

Hej ”sasURL” post innehåller hello fullständig Webbadress som innehåller SAS-token för hello Event Hub toowhich data måste publiceras. LAD kräver en SAS namngivning av en princip som aktiverar hello skicka anspråk. Ett exempel:

* Skapa ett namnområde för Händelsehubbar kallas`contosohub`
* Skapa en Händelsehubb i hello-namnområde som kallas`syslogmsgs`
* Skapa en princip för delad åtkomst på hello Event Hub med namnet `writer` som aktiverar hello skicka anspråk

Om du har skapat en SAS bra till midnatt UTC på den 1 januari 2018 kan hello sasURL värdet vara:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

Mer information om hur du genererar SAS-token för Händelsehubbar finns [webbsidan](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

#### <a name="hello-jsonblob-sink"></a>Hej JsonBlob mottagare

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

Data dirigeras tooa JsonBlob sink är lagrade i blobar i Azure-lagring. Varje instans av LAD skapar en blob varje timme för varje mottagare namn. Varje blobb innehåller alltid en syntaktiskt giltig JSON-matris med objektet. Nya poster läggs automatiskt till toohello matris. Blobbar som lagras i en behållare med samma namn som hello sink hello. hello Azure storage-regler för blob-behållarnamn tillämpas toohello namnen på JsonBlob sänkor: mellan 3 och 63 gemena alfanumeriska ASCII-tecken eller bindestreck.

## <a name="public-settings"></a>Inställningar för offentliga

Den här strukturen innehåller olika block med inställningar som styr hello information som samlas in av hello-tillägget. Varje inställningen är valfri. Om du anger `ladCfg`, måste du också ange `StorageAccount`.

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "hello storage account tooreceive data",
    "mdsdHttpProxy" : ""
}
```

Element | Värde
------- | -----
StorageAccount | hello namnet på hello lagringskonto där data skrivs av hello-tillägget. Måste vara samma namn som har angetts i hello hello [skyddade inställningarna](#protected-settings).
mdsdHttpProxy | (valfritt) Samma som i hello [skyddade inställningarna](#protected-settings). hello offentliga värde åsidosätts av hello privata värde, om ange. Placera proxyinställningar som innehåller en hemlighet, till exempel ett lösenord i hello [skyddade inställningarna](#protected-settings).

hello återstående elementen beskrivs detaljerat i följande avsnitt hello.

### <a name="ladcfg"></a>ladCfg

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

Den här valfria struktur kontroller hello insamlingen av mätvärden och loggfiler för leverans toohello Azure-tjänsten och tooother mätvärdesdata egenskaperna. Du måste ange antingen `performanceCounters` eller `syslogEvents` eller båda. Du måste ange hello `metrics` struktur.

Element | Värde
------- | -----
eventVolume | (valfritt) Kontroller hello antalet partitioner som skapas i hello lagring tabell. Måste vara något av `"Large"`, `"Medium"`, eller `"Small"`. Om inte anges är standardvärdet för hello `"Medium"`.
sampleRateInSeconds | (valfritt) hello standardintervallet mellan samling raw (unaggregated) mått. hello minsta stöds samplingsfrekvens är 15 sekunder. Om inte anges är standardvärdet för hello `15`.

#### <a name="metrics"></a>metrics

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

Element | Värde
------- | -----
resourceId | hello Azure Resource Manager resurs-ID för hello VM eller hello virtuella skala ange toowhich hello VM tillhör. Den här inställningen måste vara anges också om alla JsonBlob sink används i hello konfiguration.
scheduledTransferPeriod | hello frekvens vid vilken sammanställd är toobe beräknad och överförs tooAzure statistik, uttryckt som en är 8601 tidsintervall. hello minsta överföringsperioden är 60 sekunder, det vill säga PT1M. Du måste ange minst en scheduledTransferPeriod.

Exempel för hello mått som anges i avsnittet för hello prestandaräknarna samlas var 15: e sekund eller i hello samplingsfrekvens definieras explicit för hello räknare. Om flera scheduledTransferPeriod frekvenser visas (som hello exempel), beräknas varje aggregerings oberoende av varandra.

#### <a name="performancecounters"></a>performanceCounters

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

Det här valfria avsnittet styr hello samling mått. Rådata prover aggregeras för varje [scheduledTransferPeriod](#metrics) tooproduce dessa värden:

* medelvärde
* minsta
* Maximalt
* senaste samlas in värdet
* antal prover raw används toocompute hello aggregering

Element | Värde
------- | -----
egenskaperna | (valfritt) En kommaavgränsad lista med namnen på egenskaperna toowhich LAD skickar sammanlagda mått resultat. Alla är sammanställda publicerade tooeach visas mottagare. Se [sinksConfig](#sinksconfig). Exempel: `"EHsink1, myjsonsink"`.
typ | Identifierar hello faktiska providern av hello mått.
Klass | Identifierar hello specifikt mått i hello leverantörens namnrymd tillsammans med ”räknaren”.
Räknaren | Identifierar hello specifikt mått i hello leverantörens namnrymd tillsammans med ”class”.
counterSpecifier | Identifierar hello specifikt mått i hello Azure mått namnområde.
Villkor | (valfritt) Väljer en specifik instans av hello objektet toowhich hello mått gäller eller väljer hello sammanställning över alla instanser av objektet. Mer information finns i hello [ `builtin` måttdefinitioner](#metrics-supported-by-builtin).
sampleRate | ÄR 8601 intervallet som anger Hej då rådata prover för det här måttet har samlats in. Om inte ange intervall för insamling av hello har angetts av hello värdet för [sampleRateInSeconds](#ladcfg). hello kortaste stöds samplingsfrekvens är 15 sekunder (PT15S).
enhet | Måste vara ett av de här strängarna: ”antal”, ”byte”, ”sekunder”, ”procent”, ”CountPerSecond”, ”BytesPerSecond”, ”millisekunder”. Definierar hello enhet för hello mått. Konsumenter av hello insamlade data förväntas hello insamlade data värden toomatch denna enhet. LAD ignorerar det här fältet.
Visningsnamn | hello etikett (på hello språk som anges av hello associerade nationella inställningarna) toobe kopplade toothis data i Azure mått. LAD ignorerar det här fältet.

Hej counterSpecifier är ett valfritt ID. Konsumenter av statistik, som hello Azure portal diagram och avisering funktion, Använd counterSpecifier som hello ”nyckel” som identifierar ett mått eller en instans av ett mått. För `builtin` statistik, rekommenderar vi du använder counterSpecifier värden som börjar med `/builtin/`. Om du samlar in en specifik instans av ett mått, rekommenderar vi du bifoga hello identifierare för hello instans toohello counterSpecifier värde. Några exempel:

* `/builtin/Processor/PercentIdleTime`-Inaktivitetstid var i genomsnitt över alla kärnor
* `/builtin/Disk/FreeSpace(/mnt)`-Ledigt utrymme för hello /mnt filsystem
* `/builtin/Disk/FreeSpace`-Ledigt utrymme som var i genomsnitt över alla monterade filsystem

Varken LAD eller hello Azure-portalen förväntar hello counterSpecifier värdet toomatch alla mönster. Vara konsekvent i hur du skapar counterSpecifier värden.

När du anger `performanceCounters`, LAD skrivs alltid tooa tabell i Azure-lagring. Du kan ha hello samma data som skrivs tooJSON blobbarna och/eller Händelsehubbar, men du inaktivera inte lagra data tooa tabell. Alla instanser av hello diagnostiska tillägg konfigurerat toouse hello samma lagringskonto namn och en slutpunkt kan du lägga till sina mått och loggar toohello samma tabell. Om för många virtuella datorer skriver skriver toohello samma tabell, partition i Azure kan begränsa toothat partition. Hej eventVolume inställningen orsaker poster toobe fördelade på 1 (liten), 10 (medel) eller 100 (stor) olika partitioner. Vanligtvis är ”medel” räcker tooensure trafik begränsas inte. hello använder Azure mått av hello Azure-portalen hello data i den här tabellen tooproduce diagram eller tootrigger aviseringar. hello tabellnamnet är hello sammanfogning av dessa strängar:

* `WADMetrics`
* Hej ”scheduledTransferPeriod” för hello samman värden som lagras i hello tabell
* `P10DV2S`
* Ett datum i form av Hej ”ÅÅÅÅMMDD” som ändrar var 10: e dag

Exempel inkluderar `WADMetricsPT1HP10DV2S20170410` och `WADMetricsPT1MP10DV2S20170609`.

#### <a name="syslogevents"></a>syslogEvents

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

Det här valfria avsnittet styr hello insamling av händelser från syslog. Syslog-händelser fångas inte alls om hello avsnittet utelämnas.

Hej syslogEventConfiguration samlingen innehåller en post för varje syslog-funktion av intresse. Om minSeverity är ”NONE” för en viss funktion, eller om den och inte visas i hello element på alla, fångas inga händelser från den anläggningen.

Element | Värde
------- | -----
egenskaperna | En kommaavgränsad lista med namnen på egenskaperna toowhich enskild logg händelserna publiceras. Alla händelser matchar hello begränsningar i syslogEventConfiguration är publicerade tooeach visas mottagare. Exempel: ”EHforsyslog”
Funktionen | En lokal syslog-namn (som ”LOGGA\_användare” eller ”LOGGA\_LOCAL0”). Avsnittet Hej ”och” i hello [syslog man sidan](http://man7.org/linux/man-pages/man3/syslog.3.html) hello fullständig lista.
minSeverity | En syslog-allvarlighetsgrad (som ”LOGGA\_fel” eller ”LOGGA\_INFO”). Avsnittet hello ”nivå” i hello [syslog man sidan](http://man7.org/linux/man-pages/man3/syslog.3.html) hello fullständig lista. hello tillägget samlar in händelser skickas toohello anläggning i eller ovanför hello angetts nivå.

När du anger `syslogEvents`, LAD skrivs alltid tooa tabell i Azure-lagring. Du kan ha hello samma data som skrivs tooJSON blobbarna och/eller Händelsehubbar, men du inaktivera inte lagra data tooa tabell. hello partitionering beteendet för den här tabellen är hello detsamma som beskrevs för `performanceCounters`. hello tabellnamnet är hello sammanfogning av dessa strängar:

* `LinuxSyslog`
* Ett datum i form av Hej ”ÅÅÅÅMMDD” som ändrar var 10: e dag

Exempel inkluderar `LinuxSyslog20170410` och `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Det här valfria avsnittet styr körning av godtycklig [OMI](https://github.com/Microsoft/omi) frågor.

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

Element | Värde
------- | -----
namnområde | (valfritt) hello OMI namnområde inom vilken hello frågan ska köras. Om inget anges hello standardvärdet är ”root/scx”, implementeras av hello [System Center plattformsoberoende Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
DocumentDB | hello OMI frågan toobe utförs.
Tabell | (valfritt) hello Azure storage tabell i hello avses storage-konto (se [skyddade inställningarna](#protected-settings)).
frequency | (valfritt) hello antalet sekunder mellan körning av hello frågan. Standardvärdet är 300 (5 minuter). lägsta värdet är 15 sekunder.
egenskaperna | (valfritt) En kommaavgränsad lista över ytterligare sänkor toowhich raw mått resultat ska publiceras. Ingen aggregering av exemplen rådata beräknas genom hello tillägg eller Azure mått.

Antingen ”table” eller ”egenskaperna”, eller både och måste anges.

### <a name="filelogs"></a>fileLogs

Kontroller hello avbilda loggfiler. LAD in nya textrader som de är skrivna toohello filen och skriver dem tootable rader och/eller alla angivna sänkor (JsonBlob eller EventHub).

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

Element | Värde
------- | -----
Filen | hello fullständig sökväg till hello log file toobe bevakade och avbildas. hello pathname måste namnet på en enstaka fil. Det går inte att namnge en katalog eller innehåller jokertecken.
Tabell | (valfritt) hello Azure storage tabell i hello avses lagring konto (som anges i hello skyddade konfiguration) till vilka nya rader från hello ”stjärt” hello filen skrivs.
egenskaperna | (valfritt) En kommaavgränsad lista över ytterligare sänkor toowhich loggen rader skickas.

Antingen ”table” eller ”egenskaperna”, eller både och måste anges.

## <a name="metrics-supported-by-hello-builtin-provider"></a>Mått som stöds av hello builtin-providern

hello builtin mått providern är en källa för mått mest intressanta tooa bred uppsättning användare. De här måtten faller inom fem bred klasser:

* Processor
* Minne
* Nätverk
* Filsystem
* Disk

### <a name="builtin-metrics-for-hello-processor-class"></a>inbyggd mätvärden för hello Processor-klass

hello Processor klass av mätvärden innehåller information om processoranvändning i hello VM. Vid sammanställning procenttal är hello resultatet hello genomsnitt mellan alla processorer. I två grundläggande VM om en kärna var 100% upptagen och hello andra 100% inaktiv rapporteras hello PercentIdleTime skulle värdet vara 50. Om varje kärna 50% upptagen för hello samma period hello rapporterade resultat kan även vara 50. I en fyra kärnor virtuell dator med en kärna med 100% upptagen och hello andra inaktiv hello rapporterade PercentIdleTime är 75.

Räknaren | Betydelse
------- | -------
PercentIdleTime | Procentandel av tiden under hello aggregering fönster som processorer köra hello kernel inaktiv loop
PercentProcessorTime | Procentandelen av tid att köra en icke-inaktiv tråd
PercentIOWaitTime | Procentandelen tid som väntar på att toocomplete för i/o-åtgärder
PercentInterruptTime | Procentandelen tid som kör maskin-och programvara avbrott och DPC: er (uppskjutna proceduranrop)
PercentUserTime | Icke-inaktiv tid under hello aggregering hello procentandelen tid som ägnats åt användaren fler med normal prioritet
PercentNiceTime | Icke-inaktiv tid hello procent av nedsänkt (bra) prioritet
PercentPrivilegedTime | Icke-inaktiv tid hello procent av privilegierad (kernel)-läge

hello bör först fyra räknare sum too100%. hello senast räknare tre också summan too100%. de dela upp hello summan av PercentProcessorTime, PercentIOWaitTime och PercentInterruptTime.

Ange tooobtain ett mått samman mellan alla processorer `"condition": "IsAggregate=TRUE"`. tooobtain ett mått för en viss processor så som hello andra logisk processor för en fyra grundläggande VM, ange `"condition": "Name=\\"1\\""`. Logisk processor nummer är inom räckhåll hello `[0..n-1]`.

### <a name="builtin-metrics-for-hello-memory-class"></a>inbyggd mätvärden för hello minne klass

hello minne klass av mätvärden innehåller information om växling och växla minnesanvändning.

Räknaren | Betydelse
------- | -------
AvailableMemory | Tillgängligt fysiskt minne i MiB
PercentAvailableMemory | Tillgängligt fysiskt minne i procent av det totala minnet
UsedMemory | Använd fysiskt minne (MiB)
PercentUsedMemory | Används fysiskt minne i procent av det totala minnet
PagesPerSec | Total växling (läsa/skriva)
PagesReadPerSec | Sidorna läses säkerhetskopiering store (växlingsfil, fil, mappade filen, etc.)
PagesWrittenPerSec | Sidor som skrivs toobacking lagra (växlingsfil, mappade filen osv.)
AvailableSwap | Oanvända växlingsutrymme (MiB)
PercentAvailableSwap | Oanvända växlingsutrymme som en procentandel av totalt växlingsutrymme
UsedSwap | Använd växlingsutrymme (MiB)
PercentUsedSwap | Används växlingsutrymme som en procentandel av totalt växlingsutrymme

Mått i den här klassen har en enda instans. Hej ”villkor” attribut har inga användbara inställningar och ska uteslutas.

### <a name="builtin-metrics-for-hello-network-class"></a>inbyggd mätvärden för hello nätverket klass

hello nätverket klass av mätvärden innehåller information om nätverksaktivitet på en enskild nätverksgränssnitt sedan start. LAD visar inte gränssnittshändelsen bandbredd statistik, som kan hämtas från värden mått.

Räknaren | Betydelse
------- | -------
BytesTransmitted | Totalt antal byte som skickats sedan start
BytesReceived | Totalt antal byte som tagits emot sedan start
BytesTotal | Totalt antal byte som skickats eller tagits emot sedan start
PacketsTransmitted | Totalt antal paket som skickats sedan start
PacketsReceived | Totalt antal paket som tagits emot sedan start
TotalRxErrors | Antal mottagna fel sedan start
TotalTxErrors | Antal överföringsfel sedan start
TotalCollisions | Antal kollisioner som rapporterats av hello nätverksportar sedan start

 Även om den här klassen är instanced stöder inte LAD sparandet nätverket mått samman över alla nätverksenheter. tooobtain hello mått för ett visst gränssnitt, till exempel eth0, ange `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-hello-filesystem-class"></a>inbyggd mätvärden för hello filsystem-klass

hello Filesystem klass av mätvärden innehåller information om användning av filsystem. Absoluta och procentuella värden rapporteras som de skulle vara visas tooan vanlig användare (inte rot).

Räknaren | Betydelse
------- | -------
FreeSpace | Tillgängligt diskutrymme i byte
UsedSpace | Använda diskutrymmet i byte
PercentFreeSpace | Procent ledigt utrymme
PercentUsedSpace | Använt utrymme i procent
PercentFreeInodes | Procentandelen oanvända noder
PercentUsedInodes | Procentandelen allokerat (i används) noder summeras över alla filsystem
BytesReadPerSecond | Läsa antalet byte per sekund
BytesWrittenPerSecond | Byte som skrivs per sekund
BytesPerSecond | Byte läsas eller skrivas per sekund
ReadsPerSecond | Läsåtgärder per sekund
WritesPerSecond | Skrivåtgärder per sekund
TransfersPerSecond | Läsa eller skriva åtgärder per sekund

Sammanställda värden över alla filsystem kan erhållas genom att ange `"condition": "IsAggregate=True"`. Värden för en specifik monterade filsystem, som ”/ mnt”, kan erhållas genom att ange `"condition": 'Name="/mnt"'`.

### <a name="builtin-metrics-for-hello-disk-class"></a>inbyggd mätvärden för hello Disk-klass

hello Disk klass av mätvärden innehåller information om diskanvändning för enheten. Statistiken gäller toohello hela enheten. Om det finns flera filsystem på en enhet, är hello räknare för enheten ett effektivt sätt, samman över alla.

Räknaren | Betydelse
------- | -------
ReadsPerSecond | Läsåtgärder per sekund
WritesPerSecond | Skrivåtgärder per sekund
TransfersPerSecond | Totalt antal åtgärder per sekund
AverageReadTime | Det genomsnittliga antalet sekunder per läsning
AverageWriteTime | Det genomsnittliga antalet sekunder per skrivning
AverageTransferTime | Det genomsnittliga antalet sekunder per åtgärd
AverageDiskQueueLength | Genomsnittligt antal köade diskåtgärder
ReadBytesPerSecond | Antalet lästa byte per sekund
WriteBytesPerSecond | Antalet byte som skrivs per sekund
BytesPerSecond | Antalet byte som har lästs eller skrivits per sekund

Sammanställda värden över alla diskar kan erhållas genom att ange `"condition": "IsAggregate=True"`. Ange tooget information för en specifik enhet (till exempel/dev/sdf1), `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Installera och konfigurera LAD 3.0 via CLI

Under förutsättning att de skydda inställningarna är i hello filen PrivateConfig.json och din offentliga konfigurationsinformation finns i PublicConfig.json, kör kommandot:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

hello kommandot förutsätter att du använder hello Azure Resource Manager-läge (arm) av hello Azure CLI. tooconfigure LAD för klassisk distribution modellen (ASM) virtuella datorer, växla för ”asm”-läge (`azure config mode asm`) och utelämna hello resursgruppens namn i hello-kommandot. Mer information finns i hello [plattformsoberoende CLI dokumentationen](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>En exempelkonfiguration LAD 3.0

Baserat på hello föregående definitioner, härs exempel LAD 3.0 tilläggets konfiguration med förklaringar. tooapply det här exemplet tooyour, bör du använda namnet på ditt eget lagringskonto, SAS-token-konto och EventHubs SAS-token.

### <a name="privateconfigjson"></a>PrivateConfig.json

Konfigurera inställningarna för privat:

* ett lagringskonto
* en matchande konto-SAS-token
* flera egenskaperna (JsonBlob eller EventHubs med SAS-token)

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a>PublicConfig.json

Dessa inställningar för offentliga orsaka LAD till:

* Överför procent processortid och använda diskutrymme mått toohello `WADMetrics*` tabell
* Överföra meddelanden från syslog anläggning ”användare” och allvarlighetsgraden ”information” toohello `LinuxSyslog*` tabell
* Överför rådata OMI frågan resultat (PercentProcessorTime och PercentIdleTime) toohello med namnet `LinuxCPU` tabell
* Överför tillagda rader i filen `/var/log/myladtestlog` toohello `MyLadTestLog` tabell

Data överförs också till i varje fall:

* Azure Blob storage (behållarens namn är enligt definitionen i hello JsonBlob sink)
* EventHubs slutpunkt (som anges i hello EventHubs sink)

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

Hej `resourceId` hello konfiguration måste matcha att ange hello VM eller hello virtuella skalan.

* Azure-plattformen mått diagram och avisering vet hello resourceId av hello VM som du arbetar med. Det förväntas toofind hello data för den virtuella datorn med hjälp av hello resourceId hello sökning nyckel.
* Om du använder Azure Autoskala måste hello resourceId i hello Autoskala konfigurationen matcha hello resourceId används av LAD.
* hello resourceId är inbyggd i hello namnen på JsonBlobs som skrivits av LAD.

## <a name="view-your-data"></a>Granska dina data

Använd hello Azure portal tooview prestandadata eller Ställ in aviseringar:

![Bild](./media/diagnostic-extension/graph_metrics.png)

Hej `performanceCounters` data lagras alltid i en tabell i Azure Storage. Azure Storage-API: er är tillgängliga för många språk och plattformar.

Data som skickas tooJsonBlob sänkor lagras i BLOB i hello lagringskontonamnet i hello [skyddade inställningarna](#protected-settings). Du kan använda hello blob-data med hjälp av alla Azure Blob Storage-API: er.

Dessutom kan du använda dessa UI verktyg tooaccess hello data i Azure Storage:

* Visual Studio Server Explorer.
* [Microsoft Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/ "Azure Lagringsutforskaren").

Den här ögonblicksbild av en Microsoft Azure Lagringsutforskaren session visar hello genererats Azure Storage-tabeller och behållare från en korrekt konfigurerad LAD 3.0-tillägg på ett test VM. hello avbildningen matchar inte exakt hello [LAD 3.0 exempelkonfiguration](#an-example-lad-30-configuration).

![Bild](./media/diagnostic-extension/stg_explorer.png)

Se hello relevanta [EventHubs dokumentationen](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn hur tooconsume meddelanden publicerade tooan EventHubs slutpunkt.

## <a name="next-steps"></a>Nästa steg

* Skapa mått aviseringar i [Azure-Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) för hello mått som du samlar in.
* Skapa [övervakning diagram](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) för din mått.
* Lär dig hur för[skapa en virtuella datorns skaluppsättning](/azure/virtual-machines/linux/tutorial-create-vmss) med din mått toocontrol autoskalning.
