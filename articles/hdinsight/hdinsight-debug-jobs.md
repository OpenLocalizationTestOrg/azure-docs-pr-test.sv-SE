---
title: "Felsöka Hadoop i HDInsight: visa loggar och tolka felmeddelanden - Azure | Microsoft Docs"
description: "Läs mer om hello felmeddelanden du kan få när du administrerar HDInsight med hjälp av PowerShell och vad du kan göra toorecover."
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: mumian
documentationcenter: 
ms.assetid: 7e6ceb0e-8be8-4911-bc80-20714030a3ad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
ms.openlocfilehash: 2f12a40e9dcac32ce2e11d66d60d8b3b212ecb53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-hdinsight-logs"></a>Analysera HDInsight-loggar
Varje Hadoop-kluster i Azure HDInsight har ett Azure storage-konto som används som hello standardfilsystem. Hej lagringskonto kallas hello standardkontot för lagring. Klustret använder hello Azure Table storage och hello Blob storage på hello standard Storage-konto toostore loggar.  toofind ut hello standardkontot för lagring för klustret, se [hantera Hadoop-kluster i HDInsight](hdinsight-administer-use-management-portal.md#find-the-default-storage-account). hello loggar behåller i hello Storage-konto, även när hello kluster har tagits bort.

## <a name="logs-written-tooazure-tables"></a>Loggar som skrivs tooAzure tabeller
hello ger loggar som skrivs tooAzure tabeller en inblick i vad som händer med ett HDInsight-kluster.

När du skapar ett HDInsight-kluster skapas automatiskt 6 tabeller för Linux-baserade kluster i hello standard Table storage:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

3 tabeller skapas för Windows-baserade kluster:

* Setuplog: logg över händelser/undantag uppstod i etablering/upprättandet av HDInsight-kluster.
* hadoopinstalllog: logg över händelser/undantag uppstod när du installerar Hadoop på hello klustret. Den här tabellen kan vara användbara vid felsökning av problem relaterade tooclusters som skapats med anpassade parametrar.
* hadoopservicelog: logg över händelser/undantag som har registrerats av alla Hadoop-tjänster. Den här tabellen kan vara användbar vid felsökning av problem relaterade toojob misslyckade på HDInsight-kluster.

hello tabell filnamn **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Dessa tabeller innehåller hello följande fält:

* ClusterDnsName
* Komponentnamn
* eventTimestamp
* Värd
* MALoggingHash
* Meddelande
* N
* PreciseTimeStamp
* Roll
* RowIndex
* Klient
* TIDSSTÄMPEL
* TraceLevel

### <a name="tools-for-accessing-hello-logs"></a>Verktyg för att komma åt hello loggar
Det finns många verktyg för att komma åt data i dessa tabeller:

* Visual Studio
* Azure Lagringsutforskaren
* Power Query för Excel

#### <a name="use-power-query-for-excel"></a>Använda Power Query för Excel
Power Query kan installeras från [www.microsoft.com/en-us/download/details.aspx?id=39379](http://www.microsoft.com/en-us/download/details.aspx?id=39379). Se hello hämtningssidan för hello systemkrav

**toouse Power Query tooopen och analysera hello loggfiler**

1. Öppna **Microsoft Excel**.
2. Från hello **Power Query** -menyn klickar du på **från Azure**, och klicka sedan på **från Microsoft Azure Table storage**.
   
    ![HDInsight Hadoop Excel PowerQuery öppna Azure Table storage](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Ange hello lagringskontonamn. Detta kan vara antingen hello kort namn eller hello FQDN.
4. Ange hello lagringskontonyckel. Du bör se en lista över tabeller:
   
    ![HDInsight Hadoop-loggar som lagras i Azure Table storage](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Högerklicka på hello hadoopservicelog tabellen i hello **Navigator** fönstret och välj **redigera**. Du bör se 4 kolumner. Du kan också ta bort hello **partitionsnyckel**, **Radnyckel**, och **tidsstämpel** kolumner genom att markera dem och sedan på **ta bort kolumner** från hello alternativ i hello menyfliksområdet.
6. Klicka på hello ikonen på hello innehåll kolumnen toochoose hello kolumner som du vill tooimport hello i kalkylblad i Excel. För den här demonstrationen du har valt TraceLevel och komponentnamn: den kan ge mig grundläggande information som komponenter hade problem.
   
    ![HDInsight Hadoop-loggar Välj kolumner](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Klicka på **OK** tooimport hello data.
8. Välj hello **TraceLevel**, roll, och **komponentnamn** kolumner och klicka sedan på **Group By** kontroll menyfliken hello.
9. Klicka på **OK** i dialogrutan för hello Gruppera efter
10. Klicka på ** Tillämpa & Stäng **.

Du kan nu använda Excel toofilter och sortera efter behov. Naturligtvis, du kanske vill tooinclude andra kolumner (t.ex. meddelande) i ordning toodrill ned till problem när de inträffar, men att markera och gruppera hello kolumner som beskrivs ovan ger en hyfsad bild av vad som händer med Hadoop-tjänster. hello kan samma idé vara tillämpade toohello setuplog och hadoopinstalllog tabeller.

#### <a name="use-visual-studio"></a>Använd Visual Studio
**toouse Visual Studio**

1. Öppna Visual Studio.
2. Från hello **visa** -menyn klickar du på **Cloud Explorer**. Eller klicka bara på **CTRL +\, CTRL + X**.
3. Från **Cloud Explorer**väljer **resurstyper**.  hello andra tillgängliga alternativ är **resursgrupper**.
4. Expandera **Lagringskonton**, hello standardkontot för lagring för klustret, och sedan **tabeller**.
5. Dubbelklicka på **hadoopservicelog**.
6. Lägga till ett filter. Exempel:
   
        TraceLevel eq 'ERROR'
   
    ![HDInsight Hadoop-loggar Välj kolumner](./media/hdinsight-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Mer information om hur du skapar filter finns [konstruera Filtersträngar för hello tabelldesignern](../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-tooazure-blob-storage"></a>Loggar skriftliga tooAzure Blob Storage
[hello loggar skriftliga tooAzure tabeller](#log-written-to-azure-tables) ger en inblick i vad som händer med ett HDInsight-kluster. Dessa tabeller ger dock inte aktivitetsnivå loggarna, vilket kan vara till hjälp för att gå vidare till problem när de inträffar. tooprovide den här nästa detaljnivå, HDInsight-kluster är konfigurerat toowrite aktivitet loggar tooyour Blob Storage-konto för alla jobb som skickas via Templeton. Praktiskt taget, innebär detta jobb som skickas med hello Microsoft Azure PowerShell-cmdlets eller hello .NET jobbet skicka API: er, inte de jobb som skickats via RDP/Kommandotolken-Command-Line åtkomst toohello klustret. 

tooview hello loggar finns [åtkomst YARN programloggar på Linux-baserade HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md).

Läs mer om programloggarna [förenklad hantering av användare loggar och åtkomst i YARN](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

## <a name="view-cluster-health-and-job-logs"></a>Visa hälso- och loggar för kluster
### <a name="access-hadoop-ui"></a>Åtkomst till Hadoop-Gränssnittet
Klicka på ett HDInsight-kluster namn tooopen hello klusterbladet hello Azure-portalen. Hello kluster-bladet, klickar du på **instrumentpanelen**.

![Starta instrumentpanelen klustret](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard.png)

När du uppmanas ange administratörsautentiseringsuppgifter för hello-klustret. I hello fråga-konsol som öppnas, klickar du på **Hadoop UI**.

![Starta Användargränssnittet för Hadoop](./media/hdinsight-debug-jobs/hdi-debug-launch-dashboard-hadoop-ui.png)

### <a name="access-hello-yarn-ui"></a>Komma åt hello Yarn-Användargränssnittet
Klicka på ett HDInsight-kluster namn tooopen hello klusterbladet hello Azure-portalen. Hello kluster-bladet, klickar du på **instrumentpanelen**. När du uppmanas ange administratörsautentiseringsuppgifter för hello-klustret. I hello fråga-konsol som öppnas, klickar du på **YARN-Användargränssnittet**.

Du kan använda hello YARN-Användargränssnittet toodo hello följande:

* **Hämta status för klustret**. Från hello till vänster och expanderar **klustret**, och klicka på **om**. Den här finns kluster statusinformation som totalt allokerat minne, kärnor används, status för resurshanteraren för hello kluster, kluster version osv.
  
    ![Starta instrumentpanelen klustret](./media/hdinsight-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Hämta status för noden**. Från hello till vänster och expanderar **klustret**, och klicka på **noder**. Detta visar alla hello-noder i hello kluster, HTTP-adressen för varje nod, resurser allokerade tooeach nod och så vidare.
* **Övervaka jobbstatus**. Från hello till vänster och expanderar **klustret**, och klicka sedan på **program** toolist alla hello jobb i hello klustret. Om du vill toolook på jobb i ett visst tillstånd (till exempel nya, Skickat, körs osv.), klicka på hello länken under **program**. Du kan ytterligare Klicka hello jobbet namnet toofind mer information om hello jobbet sådana inklusive hello utdata, loggar, osv.

### <a name="access-hello-hbase-ui"></a>Komma åt hello HBase UI
Klicka på ett HDInsight HBase-kluster namn tooopen hello klusterbladet hello Azure-portalen. Hello kluster-bladet, klickar du på **instrumentpanelen**. När du uppmanas ange administratörsautentiseringsuppgifter för hello-klustret. I hello fråga-konsol som öppnas, klickar du på **HBase UI**.

## <a name="hdinsight-error-codes"></a>Felkoder för HDInsight
hello felmeddelanden i det här avsnittet finns toohelp hello användare av Hadoop i Azure HDInsight förstå olika feltillstånden som de kan stöta på när du administrerar hello-tjänsten med hjälp av Azure PowerShell och tooadvise dem på hello steg som kan tas toorecover från hello-fel.

Vissa av dessa felmeddelanden kan också visas i hello Azure-portalen när det används toomanage HDInsight-kluster. Andra felmeddelanden som kan uppstå, men det finns mindre detaljerad på grund av toohello begränsningar på hello vidtar åtgärder i den här kontexten. Andra felmeddelanden tillhandahålls i hello sammanhang där hello minskning är uppenbara. 

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Beskrivning**: Ange Azure SQL database-information för minst en komponent i ordning toouse anpassade inställningar för Hive och Oozie metastores.
* **Minskning**: hello användare behöver toosupply en giltig SQL Azure metastore och försök igen hello förfrågan.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Beskrivning**: gick inte att skapa kluster i regionen *nameOfYourRegion*. Använd ett giltigt HDInsight-regionen och försök igen med begäran.
* **Minskning**: kunden ska skapa hello klustret region som för närvarande stöder dem: Sydostasien, västra Europa, Norra Europa, östra USA eller västra USA.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Beskrivning**: hello-servern kunde inte hitta hello begärda posten för klustret.  
* **Minskning**: försök hello igen.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Beskrivning**: klustret DNS-namnet *yourDnsName* är ogiltig. Kontrollera att namnet börjar och slutar med alfanumeriska och kan endast innehålla '-' specialtecken  
* **Minskning**: Kontrollera att du har använt ett giltigt DNS-namn för klustret som börjar och slutar med alfanumeriska och innehåller ingen särskild tecken än hello dash '-' och försök sedan utföra hello igen.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Beskrivning**: klusternamnet *yourClusterName* är inte tillgänglig. Välj ett annat namn.  
* **Minskning**: hello användaren måste ange ett klusternamn som är unikt och inte finns och försök. Om hello användaren använder hello Portal, skapa hello UI meddelar dem om ett klusternamn redan används under hello steg.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Beskrivning**: klustret lösenordet är ogiltigt. Lösenordet måste vara minst 10 tecken och måste innehålla minst en siffra, versal bokstav, gemen bokstav och specialtecken utan blanksteg och får inte innehålla hello användarnamn som en del av den.  
* **Minskning**: Ange ett giltigt kluster lösenord och försök utföra hello igen.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Beskrivning**: klustret användarnamnet är ogiltigt. Kontrollera att användarnamnet inte innehålla specialtecken eller blanksteg.  
* **Minskning**: Ange ett giltigt kluster-användarnamn och försök utföra hello igen.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Beskrivning**: klustret DNS-namnet *yourDnsClusterName* är ogiltig. Kontrollera att namnet börjar och slutar med alfanumeriska och kan endast innehålla '-' specialtecken  
* **Minskning**: Ange ett giltigt användarnamn för DNS-kluster och försök utföra hello igen.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Beskrivning**: behållarnamn i URI: N *yourcontainerURI* och DNS-namnet *yourDnsName* i begäran innehållet måste vara hello samma.  
* **Minskning**: se till att din behållaren namn och DNS-namn är hello samma och försök utföra hello igen.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Beskrivning**: Ogiltig klusterkonfigurationen. Det går inte toofind alla datadefinitioner nod i noden storlek.  
* **Minskning**: försök hello igen.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Beskrivning**: borttagningen av distributionen misslyckades för hello kluster  
* **Minskning**: försök hello borttagningsåtgärd.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Beskrivning**: tjänsten konfigurationsfel. Information om nödvändiga DNS-mappning inte hittades.  
* **Minskning**: ta bort klustret och skapa ett nytt kluster.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Beskrivning**: duplicera klustret behållaren skapa försök. Det finns en post för *nameOfYourContainer* men Etags matchar inte.
* **Minskning**: Ange ett unikt namn för hello-behållaren och försök igen hello Skapa-åtgärd.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Beskrivning**: värdtjänsten *nameOfYourHostedService* innehåller redan ett kluster. En värdbaserad tjänst får inte innehålla flera kluster  
* **Minskning**: hello-värdkluster i en annan värdtjänsten.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Beskrivning**: hello-servern kunde inte uppdatera hello Klusterdistribution hello tillstånd.  
* **Minskning**: försök hello igen. Om det händer flera gånger, kontakta Kundsupporten.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Beskrivning**: klustret *yourClusterName* har tagits bort som en del av underhåll. Du måste skapa hello klustret.
* **Minskning**: återskapa hello klustret.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Beskrivning**: Ogiltig klusterkonfigurationen. Konfiguration av nödvändiga huvudnod hittades inte i noden storlekar.
* **Minskning**: försök hello igen.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Beskrivning**: Det gick inte toocreate värdtjänsten *nameOfYourHostedService*. Försök att utföra begäran.  
* **Minskning**: försök hello begäran.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Beskrivning**: värdtjänsten *nameOfYourHostedService* har redan en Produktionsdistribution. En värdbaserad tjänst får inte innehålla flera Produktionsdistribution. Hello-begäran med ett annat klusternamn och försök.
* **Minskning**: Använd ett annat klusternamn och försök hello-begäran.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Beskrivning**: värdtjänsten *nameOfYourHostedService* för hello klustret inte kunde hittas.  
* **Minskning**: om hello klustret är i feltillstånd, ta bort den och försök sedan igen.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Beskrivning**: värdtjänsten *nameOfYourHostedService* har ingen associerad distribution.  
* **Minskning**: om hello klustret är i feltillstånd, ta bort den och försök sedan igen.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Beskrivning**: hello SubscriptionId *yourSubscriptionId* saknar kärnor vänstra toocreate klustret *yourClusterName*. Obligatoriskt: *resourcesRequired*, tillgänglig: *resourcesAvailable*.  
* **Minskning**: Frigör resurser i din prenumeration eller öka hello resurser tillgängliga toohello prenumerationen och försök toocreate hello klustret igen.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Beskrivning**: prenumerations-ID *yourSubscriptionId* saknar kvoten för ett nytt kluster i HostedService toocreate *yourClusterName*.  
* **Minskning**: Frigör resurser i din prenumeration eller öka hello resurser tillgängliga toohello prenumerationen och försök toocreate hello klustret igen.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Beskrivning**: hello servern påträffade ett internt fel. Försök att utföra begäran.  
* **Minskning**: försök hello begäran.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Beskrivning**: Azure-lagringsplats *dataRegionName* är inte en giltig plats. Kontrollera att hello region är korrekt och försök igen med begäran.
* **Minskning**: Välj en lagringsplats som stöder HDInsight, kontrollera att klustret är samplacerade och försök hello igen.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Beskrivning**: Ogiltig VM-storlek för datanoder. Endast 'stora VM-storleken stöds för alla datanoder.  
* **Minskning**: Ange hello stöds nodstorlek för hello datanod och försök utföra hello igen.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Beskrivning**: Ogiltig VM-storlek för huvudnod. Endast 'Extrastora VM-storlek stöds för huvudnod.  
* **Minskning**: Ange hello stöds nodstorlek för hello huvudnod och försök utföra hello igen

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Beskrivning**: prenumerations-ID *yourSubscriptionId* används inte har tillräckliga behörigheter tooexecute borttagningsåtgärd för klustret *yourClusterName*.  
* **Minskning**: om hello klustret är i feltillstånd, släpp den och försök sedan igen.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Beskrivning**: externa blob-behållaren lagringskontonamnet *yourContainerName* är ogiltig. Kontrollera att namnet börjar med en bokstav och innehåller bara gemena bokstäver, siffror och bindestreck.  
* **Minskning**: Ange en giltig blob-behållaren lagringskontonamnet och försök utföra hello igen.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Beskrivning**: konfiguration för externa lagringskontot *yourStorageAccountName* är obligatoriska toohave information om hemlig nyckel toobe uppsättningen.  
* **Minskning**: Ange en giltig hemlig nyckel för hello storage-konto och försök utföra hello igen.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Beskrivning**: versionshuvudet *yourVersionHeader* är inte giltigt format för åååå-mm-dd.  
* **Minskning**: Ange ett giltigt format för hello-versionshuvud och försök hello-begäran.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Beskrivning**: Ogiltig klusterkonfigurationen. Hitta mer än en huvudnod konfiguration.  
* **Minskning**: redigera hello konfiguration så att bara en huvudnod har angetts.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Beskrivning**: hello-åtgärden slutfördes inte inom tillåtet tid hello eller hello maximalt antal försök möjligt. Försök att utföra begäran.  
* **Minskning**: försök hello begäran.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Beskrivning**: parametern *yourParameterName* får inte vara null eller tomt.  
* **Minskning**: Ange ett giltigt värde för parametern hello.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Beskrivning**: en eller flera av hello klustret skapas begäran indata är inte giltig. Kontrollera att hello indatavärden är korrekta och försök igen med begäran.  
* **Minskning**: Kontrollera hello indatavärden är korrekta och försök igen med begäran.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Beskrivning**: Region-funktioner som är inte tillgängliga för region *yourRegionName* prenumerations-ID och *yourSubscriptionId*.  
* **Minskning**: Ange en region som har stöd för HDInsight-kluster. hello stöds offentligt områden är: Sydostasien, västra Europa, Norra Europa, östra USA eller västra USA.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Beskrivning**: lagringskonto *yourStorageAccountName* finns i regionen *currentRegionName*. Det ska vara samma som hello klustret region *yourClusterRegionName*.  
* **Minskning**: ange antingen ett lagringskonto hello samma region som klustret är i eller om data finns redan i hello storage-konto kan du skapa ett nytt kluster i hello i samma region som hello befintligt lagringskonto. Om du använder hello Portal meddelar hello UI dem om det här problemet i förväg.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Beskrivning**: anges prenumerations-ID *yourSubscriptionId* är inte aktiv.  
* **Minskning**: aktivera prenumerationen igen eller skaffa en giltig prenumeration.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Beskrivning**: prenumerations-ID *yourSubscriptionId* kunde inte hittas.  
* **Minskning**: Kontrollera att ditt prenumerations-ID är giltigt och försök utföra hello igen.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Beskrivning**: Det gick inte tooresolve DNS *yourDnsUrl*. Kontrollera att hello fullständigt kvalificerade URL för hello slutpunkt för blob tillhandahålls.  
* **Minskning**: Ange en giltig blob-URL. hello URL måste vara fullständigt giltiga, inklusive från och med *http://* och slutar på *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Beskrivning**: tooverify platsen för resurs *yourDnsUrl*. Kontrollera att hello fullständigt kvalificerade URL för hello slutpunkt för blob tillhandahålls.  
* **Minskning**: Ange en giltig blob-URL. hello URL måste vara fullständigt giltiga, inklusive från och med *http://* och slutar på *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Beskrivning**: Version-funktioner som är inte tillgängliga för version *specifiedVersion* prenumerations-ID och *yourSubscriptionId*.  
* **Minskning**: välja en version som är tillgänglig och försök utföra hello igen.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Beskrivning**: Version *specifiedVersion* stöds inte.
* **Minskning**: välja en version som stöds och försök utföra hello igen.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Beskrivning**: Version *specifiedVersion* är inte tillgänglig i Azure-region *specifiedRegion*.  
* **Minskning**: välja en version som stöds i hello område som har angetts och försök utföra hello igen.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Beskrivning**: Ogiltig klusterkonfigurationen. Nödvändiga WASB kontokonfigurationen hittades inte i externa konton.  
* **Minskning**: Kontrollera att hello kontot finns och har angetts korrekt i konfigurationen och försök igen hello-åtgärd.

## <a name="next-steps"></a>Nästa steg
* [Använda Ambari Views toodebug Tez jobb i HDInsight](hdinsight-debug-ambari-tez-view.md)
* [Aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Hantera HDInsight-kluster med hjälp av hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md)

