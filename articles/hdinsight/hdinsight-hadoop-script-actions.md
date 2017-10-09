---
title: aaaScript utveckling med HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toocustomize Hadoop-kluster med skriptåtgärder. Skriptåtgärder kan vara används tooinstall ytterligare programvara som körs på en Hadoop-kluster eller toochange hello konfiguration av program på ett kluster."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 4fc3a389df8a003f7129ab00b4cd9bc7ad81a419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Utveckla skriptåtgärd skript för HDInsight Windows-baserade kluster
Lär dig hur toowrite skriptåtgärd skript för HDInsight. Information om hur du använder skriptåtgärd skript finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md). Hello samma artikel skrivna för Linux-baserade HDInsight-kluster finns i [utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions-linux.md).



> [!IMPORTANT]
> hello stegen i det här dokumentet endast fungerar för Windows-baserade HDInsight-kluster. HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Information om användning av skriptåtgärder med Linux-baserade kluster finns i [utveckling av skriptåtgärder med HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).
>
>



Skriptåtgärder kan vara används tooinstall ytterligare programvara som körs på en Hadoop-kluster eller toochange hello konfiguration av program på ett kluster. Med skriptåtgärder avses skript som körs på hello klusternoder när HDInsight-kluster har distribuerats och de utförs när noderna i klustret hello Slutför HDInsight-konfigurationen. En skriptåtgärd körs under kontot systemadministratörsprivilegier och ger fullständig behörighet toohello klusternoder. Varje kluster kan anges med en lista över skript åtgärder toobe körs i hello ordning som de har angetts.

> [!NOTE]
> Om du får följande felmeddelande hello:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage: hello termen ”spara HDIFile' identifieras inte som hello namnet för en cmdlet, funktion, skriptfilen eller körbart program. Hello stavning hello namn eller om en sökväg ingick Kontrollera hello sökvägen är korrekt och försök igen.
> Det beror på att du inte inkludera hello hjälpmetoder.  Se [hjälpmetoder för anpassade skript](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).
>
>

## <a name="sample-scripts"></a>Exempelskript
För att skapa HDInsight-kluster på Windows operativsystem, är hello skriptåtgärd Azure PowerShell-skript. hello är följande skript ett exempel för att konfigurera hello plats konfigurationsfiler:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable tooconfigure $ConfigFileName because it is not part of hello HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

hello skriptet använder fyra parametrar, hello konfigurationsfilnamnet, hello-egenskap som du vill använda toomodify hello-värde som du vill tooset och en beskrivning. Exempel:

    hive-site.xml hive.metastore.client.socket.timeout 90

Dessa parametrar anger hello hive.metastore.client.socket.timeout värdet too90 i hello hive-site.xml-filen.  hello standardvärdet är 60 sekunder.

Det här exempelskriptet finns även på [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight tillhandahåller flera skript tooinstall ytterligare komponenter i HDInsight-kluster:

| Namn | Skript |
| --- | --- |
| **Installera Spark** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Se [installera och använda Spark i HDInsight-kluster][hdinsight-install-spark]. |
| **Installera R** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Se [installera och använda R i HDInsight-kluster][hdinsight-r-scripts]. |
| **Installera Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md). |
| - **Installera Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md). |

Skriptåtgärder kan distribueras från hello Azure-portalen, Azure PowerShell eller med hjälp av hello HDInsight .NET SDK.  Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize].

> [!NOTE]
> hello exempelskript fungerar bara med HDInsight-kluster av version 3.1 eller senare. Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Hjälpmetoder för anpassade skript
Skriptet åtgärd helper metoder är verktyg som du kan använda vid skrivning till anpassade skript. Dessa metoder är definierade i [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), och kan ingå i ditt skript med hjälp av hello följande exempel:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module toomake writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed tooload HDInsightUtilities module, exiting ...";
        exit;
    }

Här följer hello hjälpmetoder som tillhandahålls av skriptet:

| Hjälpmetod | Beskrivning |
| --- | --- |
| **Spara HDIFile** |Hämta en fil från hello angetts identifierare URI (Uniform Resource) tooa plats på hello lokal disk som är associerad med kluster för hello Azure VM noder tilldelade toohello. |
| **Expandera HDIZippedFile** |Packa upp ZIP. |
| **Anropa HDICmdScript** |Köra ett skript från cmd.exe. |
| **Skriv HDILog** |Skriva utdata från hello anpassade skript som används för en skriptåtgärd. |
| **Get-tjänster** |Hämta en lista över tjänster som körs på datorn hello där hello skriptet körs. |
| **Get-Service** |Med hello specifika namn som indata och få detaljerad information för en specifik tjänst (namn på tjänst, process-ID, tillstånd, etc.) på hello datorn där hello skriptet körs. |
| **Get-HDIServices** |Hämta en lista över HDInsight-tjänster som körs på datorn hello där hello skriptet körs. |
| **Get-HDIService** |Med hello specifika HDInsight tjänstnamn som indata, får du detaljerad information för en specifik tjänst (namn på tjänst, process-ID, tillstånd, etc.) på hello datorn där hello skriptet körs. |
| **Get-ServicesRunning** |Hämta en lista över tjänster som körs på hello dator där hello skriptet körs. |
| **Get-ServiceRunning** |Kontrollera om en specifik tjänst (efter namn) körs på hello dator där hello skriptet körs. |
| **Get-HDIServicesRunning** |Hämta en lista över HDInsight-tjänster som körs på datorn hello där hello skriptet körs. |
| **Get-HDIServiceRunning** |Kontrollera om en specifik HDInsight-tjänst (efter namn) körs på hello dator där hello skriptet körs. |
| **Get-HDIHadoopVersion** |Hämta hello Hadoop installeras på hello dator där hello skriptet körs. |
| **Testa IsHDIHeadNode** |Kontrollera om en huvudnod hello datorn där hello skriptet körs. |
| **Testa IsActiveHDIHeadNode** |Kontrollera om en aktiv huvudnod hello datorn där hello skriptet körs. |
| **Testa IsHDIDataNode** |Kontrollera att hello datorn där hello skriptet körs är en datanod. |
| **Redigera HDIConfigFile** |Redigera hello config filer hive-site.xml core-site.xml, hdfs-site.xml, mapred site.xml eller yarn-site.xml. |

## <a name="best-practices-for-script-development"></a>Metodtips för skriptutveckling av
När du utvecklar ett anpassat skript för ett HDInsight-kluster finns flera bästa praxis tookeep i åtanke:

* Sök efter hello Hadoop-version

    Endast HDInsight version 3.1 (Hadoop 2.4) och senare stöd med skriptåtgärder tooinstall anpassade komponenter i ett kluster. I ett skript, måste du använda hello **Get-HDIHadoopVersion** helper metod toocheck hello Hadoop version innan du fortsätter med att utföra andra uppgifter i hello skript.
* Ange stabil länkar tooscript resurser

    Användare bör se till att alla hello skript och andra artefakter som används i hello anpassning av ett kluster förblir tillgängliga i hela hello livstid hello klustret och att hello versioner av dessa filer inte ändrar hello tidsåtgång för. Dessa resurser krävs om hello återställning av avbildning av noderna i klustret hello krävs. hello bästa praxis är toodownload och arkivera allt innehåll i ett lagringskonto som hello användarkontroller. Detta kan vara hello standardkontot för lagring eller någon av hello ytterligare lagringskonton på hello tidpunkten för distribution av anpassade kluster.
    I hello Spark och R anpassade kluster exempel anges i dokumentationen för hello t.ex, vi har gjort en lokal kopia av hello resurser i det här lagringskontot: https://hdiconfigactions.blob.core.windows.net/.
* Se till att hello kluster anpassning skriptet idempotent

    Förväntat att hello noder i ett HDInsight-kluster är avbildade under hello klustrets livslängd. hello klustret anpassning skript körs när ett kluster avbildade. Det här skriptet måste vara utformad toobe idempotent i hello mening att hello skript vid återställning av avbildning, bör kontrollera hello klustret returneras toohello samma anpassade tillståndet den var i efter hello skriptet kördes för hello första gången när hello klustret har från början Skapa. Till exempel om ett anpassat skript för ett program på D:\AppLocation har installerats på först kör och sedan på varje efterföljande körning vid återställning av avbildning, hello skriptet ska kontrollera om hello program finns i hello D:\AppLocation plats innan du fortsätter med andra stegen i hello skript.
* Installera komponenter för anpassade hello optimala plats

    Om klusternoderna är avbildade kan hello C:\ resurs och D:\ systemenhet formateras om, vilket resulterar i hello förlust av data och program som har installerats på dessa enheter. Detta kan också inträffa om en virtuell Azure-dator (VM)-nod som är en del av hello kluster kraschar och ersätts av en ny nod. Du kan installera komponenterna på hello D:\ enhet eller hello C:\apps plats på hello klustret. Alla andra platser på hello enhet C:\ är reserverade. Ange hello platsen för program eller bibliotek toobe som installerats i hello kluster anpassning skriptet.
* Garantera hög tillgänglighet för hello kluster-arkitektur

    HDInsight har ett aktivt-passivt arkitektur för hög tillgänglighet, i vilken en huvudnod är i aktivt läge (där hello HDInsight tjänster som körs) och hello andra huvudnod är i vänteläge (i vilken HDInsight tjänster inte körs). hello noder växla mellan lägena aktiva och passiva om HDInsight tjänster avbryts. Om en skriptåtgärd är används tooinstall tjänster på båda huvudnoderna för hög tillgänglighet, Observera att hello HDInsight mekanism för växling vid fel inte kan tooautomatically misslyckas över dessa användare installerade tjänster. Så användaren installerade tjänster på HDInsight huvudnoderna som förväntade toobe hög tillgänglighet måste antingen ha sina egna mekanism för växling vid fel om i aktivt-passivt läge eller vara i läget för aktiv-aktiv.

    Ett HDInsight-skriptåtgärder kommando som körs på båda huvudnoderna när hello huvudnod rollen har angetts som ett värde i hello *ClusterRoleCollection* parameter. Så när du skapar ett anpassat skript, se till att skriptet är medveten om den här installationen. Du bör inte köra problem där hello samma tjänster är installerade och igång på både hello huvudnoderna och hamnar konkurrerar med varandra. Tänk också på att data går förlorade vid återställning av avbildning, så att programvaran via skriptåtgärd har toobe flexibel toosuch händelser. Program ska vara utformad toowork med hög tillgänglighet data som distribueras över flera noder. Observera att du kan avbildade upp till 1/5 hello noder i ett kluster på hello samtidigt.
* Konfigurera hello anpassade komponenter toouse Azure Blob storage

    hello anpassade komponenter som installeras på hello klusternoder kan ha en standard configuration toouse Hadoop Distributed File System (HDFS) lagring. I stället bör du ändra hello configuration toouse Azure Blob storage. På ett kluster avbildningsåterställning hello HDFS hämtar filsystemet och du skulle förlora alla data som lagras där. Azure Blob storage i stället garanterar att dina data bevaras.

## <a name="common-usage-patterns"></a>Vanliga användningsmönster
Det här avsnittet ger vägledning om att implementera några hello vanliga användningsmönster som som kan uppstå vid skrivning till ett eget skript.

### <a name="configure-environment-variables"></a>Konfigurera miljövariabler
I skriptutveckling du ofta hello måste tooset miljövariabler. Exempelvis är mest sannolika scenariot när du hämtar en binär från en extern plats, installera den på hello klustret och Lägg till hello plats där det är installerade tooyour 'PATH'-miljövariabeln. hello följande utdrag visar hur tooset miljövariabler i hello anpassat skript.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Den här instruktionen anger hello miljövariabeln **MDS_RUNNER_CUSTOM_CLUSTER** toohello värdet 'true' och även hello omfånget för den här variabeln toobe datoromfattande. Ibland är det viktigt att miljövariablerna anges vid hello lämplig omfattning – dator eller användare. Se [här] [ 1] för mer information om hur du anger miljövariabler.

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a>Åtkomst till toolocations där hello anpassade skript lagras
Skript som används toocustomize en tooeither för klustret måste finnas i hello standardkontot för lagring för hello kluster eller i en offentlig skrivskyddad behållare för andra storage-konto. Om skriptet har åtkomst till resurser som finns någon annanstans dessa måste toobe i en offentligt tillgänglig (minst offentliga skrivskyddad). Du kanske exempelvis vill tooaccess en fil och sparar den hello SaveFile HDI-kommandot.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

I det här exemplet måste du kontrollera hello behållaren somecontainer om du i storage-konto 'somestorageaccount' är allmänt tillgänglig. I annat fall utlöser ett undantag 'Gick inte att hitta' hello skript och misslyckas.

### <a name="pass-parameters-toohello-add-azurermhdinsightscriptaction-cmdlet"></a>Skicka parametrar toohello Lägg till AzureRmHDInsightScriptAction cmdlet
toopass flera parametrar toohello Lägg till AzureRmHDInsightScriptAction cmdlet, behöver du tooformat hello sträng värdet toocontain alla parametrar för hello skript. Exempel:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

eller

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Utlös undantag för misslyckade Klusterdistribution
Om du vill tooget korrekt meddelande om hello faktum att klustret anpassning slutfördes inte som förväntat, det är viktigt toothrow ett undantag och misslyckas hello klustret skapas. Du kanske exempelvis vill tooprocess en fil om den finns och hantera hello fel fall där hello-filen inte finns. Detta skulle se till att hello skriptet avslutas utan problem och hello tillståndet för hello kluster kallas korrekt. hello följande kodavsnitt ger ett exempel på hur tooachieve detta:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

I det här kodstycket om hello-filen inte finns, skulle det leda tooa tillstånd där hello skriptet faktiskt avslutas korrekt efter utskrift hello felmeddelande och hello klustret når körtillstånd under förutsättning att den ”” slutförts anpassningsprocess av klustret. Om du vill toobe korrekt meddelande om hello faktum att klustret anpassning i stort sett slutfördes inte som förväntat på grund av en fil som saknas, det är mer lämpliga toothrow ett undantag och misslyckas hello klustret anpassning steg. tooachieve detta måste du använda hello följande exempel kodstycke i stället.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Checklista för distribution av en skriptåtgärd
Här är hello steg som vi har tagit när du förbereder toodeploy dessa skript:

1. Placera hello-filer som innehåller hello anpassade skript på en plats som kan nås av hello klusternoder under distributionen. Detta kan vara något av hello standard eller ytterligare lagringskonton som anges när hello Klusterdistribution och andra offentligt tillgänglig lagringsbehållaren.
2. Lägga till kontroller i skript toomake till att de kör idempotently, så att hello skript kan köras flera gånger på hello samma nod.
3. Använd hello **Write-Output** Azure PowerShell-cmdlet tooprint tooSTDOUT samt STDERR. Använd inte **Write-Host**.
4. Använda en tillfällig mapp, till exempel $env: TEMP tookeep hello hämtade filen som används av hello skript och sedan rensa dem efter skript har körts.
5. Installera anpassade programvara på D:\ eller C:\apps. Andra platser på hello enhet C: ska inte användas som de är reserverade. Observera att installera filer på hello C: enhet utanför hello C:\apps mappen kan resultera i installationsfel under reimages för hello-nod.
6. Du kanske vill toorestart HDInsight services så att de kan hämta OS-nivå inställningar, till exempel hello miljövariabler som anges i hello skript i hello händelse OS inställningar eller Hadoop service configuration-filer har ändrats.

## <a name="debug-custom-scripts"></a>Felsöka anpassade skript
hello skript felloggar lagras tillsammans med andra utdata i hello standardkontot för lagring som du angett för hello klustret när skapandet. hello loggfilerna lagras i en tabell med namnet hello *u < \cluster-name-fragment >< \time-stamp > setuplog*. Det här är sammanställda loggar som har poster från alla hello noder (huvudnod och arbetarnoder) på vilken hello skriptet körs i hello kluster.
Ett enkelt sätt toocheck hello loggar är toouse HDInsight Tools för Visual Studio. Installera hello-verktyg finns [komma igång med Visual Studio Hadoop-verktyg för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)

**toocheck hello logg med Visual Studio**

1. Öppna Visual Studio.
2. Klicka på **visa**, och klicka sedan på **Server Explorer**.
3. Högerklicka på ”Azure”, klicka på Anslut för**Microsoft Azure-prenumerationer**, och ange dina autentiseringsuppgifter.
4. Expandera **lagring**, expandera hello Azure storage-konto som används som hello standardfilsystem, expandera **tabeller**, och dubbelklicka sedan på hello tabellnamn.

Du kan också fjärråtkomst till hello klustret noder toosee STDOUT- och STDERR för anpassade skript. hello loggar på varje nod är särskilda endast toothat noder och loggas i **C:\HDInsightLogs\DeploymentAgent.log**. Dessa loggfilerna registrerar alla utdata från hello anpassade skript. Ett exempel loggen kodstycke för en Spark skriptåtgärd ser ut så här:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


Det är tydligt att hello Spark skriptåtgärd har körts på hello virtuella datorn med namnet HEADNODE0 och att inga undantag utlöstes under körning av hello i den här loggen.

I hello händelse som ett körningsfel inträffar, finns hello utdata som beskriver det också i loggfilen. hello informationen i dessa loggar är användbar vid felsökning av problem med skript som kan uppstå.

## <a name="see-also"></a>Se även
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]
* [Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]
* [Installera och använda R i HDInsight-kluster][hdinsight-r-scripts]
* [Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).
* [Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
