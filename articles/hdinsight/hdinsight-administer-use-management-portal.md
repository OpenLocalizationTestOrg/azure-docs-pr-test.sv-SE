---
title: aaaManage Windows-baserade Hadoop-kluster i HDInsight med hello Azure-portalen | Microsoft Docs
description: "Lär dig hur tooadminister HDInsight Service. Skapa ett HDInsight-klustret, öppna hello interaktiva JavaScript-konsolen och öppna hello Hadoop kommandokonsolen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: a237726b0e37a08005ce22e96581739e93edb050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a>Hantera Windows-baserade Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen

Med hjälp av hello [Azure-portalen][azure-portal], du kan skapa Windows-baserade Hadoop-kluster i Azure HDInsight, ändra Hadoop användarlösenord och aktivera Remote Desktop Protocol (RDP) så att du kan komma åt hello Hadoop kommandokonsolen på hello klustret.

hello information i den här artikeln gäller bara tooWindow-baserade HDInsight-kluster. Information om hur du hanterar Linux-baserade kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-portal-linux.md).

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Krav

Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure Storage-konto** -ett HDInsight-kluster använder en Azure Blob storage-behållare som hello standardfilsystem. Mer information om hur Azure Blob storage ger en sömlös upplevelse med HDInsight-kluster finns [använda Azure Blob Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md). Mer information om hur du skapar ett Azure Storage-konto finns [hur tooCreate ett Lagringskonto](../storage/common/storage-create-storage-account.md).

## <a name="open-hello-portal"></a>Öppna hello Portal
1. Logga in för[https://portal.azure.com](https://portal.azure.com).
2. När du har öppnat hello-portalen kan du:

   * Klicka på **ny** från hello vänstra menyn toocreate ett nytt kluster:

       ![ny knapp för HDInsight-kluster](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Klicka på **HDInsight-kluster** hello vänstra menyn.

       ![Azure portal HDInsight-kluster-knappen](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Om **HDInsight** inte visas i hello vänstra menyn klickar du på **Bläddra**.

     ![Azure portal knappen Bläddra klustret.](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Skapa kluster
Anvisningar hello skapas med hjälp av hello Portal finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight fungerar med en bred Hadoop-komponenter. Hello lista hello-komponenter som har verifierats, och som stöds finns i [vilken version av Hadoop finns i Azure HDInsight](hdinsight-component-versioning.md). Du kan anpassa HDInsight med något av följande alternativ för hello:

* Använd skriptåtgärder toorun anpassade skript som kan anpassa ett kluster tooeither ändrar klusterkonfigurationen eller installera anpassade komponenter, till exempel Giraph eller Solr. Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md).
* Använd hello anpassning Klusterparametrar i hello HDInsight .NET SDK eller Azure PowerShell när klustret skapas. De här konfigurationsändringarna bevaras sedan via hello livstid hello kluster och påverkas inte av klustret nod reimages som Azure-plattformen utför med jämna mellanrum för underhåll. Mer information om hur du använder hello Klusterparametrar anpassning finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).
* Vissa inbyggda Java-komponenter som Mahout och kaskad, kan köras på hello klustret som JAR-filer. Dessa JAR-filer kan vara distribuerade tooAzure Blob storage och skicka tooHDInsight kluster via Hadoop jobbet skicka mekanismer. Mer information finns i [skicka Hadoop-jobb via programmering](hdinsight-submit-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > Om du har problem med distribution av JAR-filer tooHDInsight kluster eller anropa JAR-filer på HDInsight-kluster, kontakta [Microsoft-supporten](https://azure.microsoft.com/support/options/).
  >
  > Sammanhängande stöds inte av HDInsight och är inte tillgänglig för Microsofts Support. Listor med stödda komponenter finns [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight](hdinsight-component-versioning.md).
  >
  >

Anpassade programvara installeras på hello kluster med hjälp av anslutning till fjärrskrivbord stöds inte. Du bör inte lagra filer för hello enheter på hello huvudnod, eftersom de kommer att förloras om du behöver toore-skapa hello kluster. Vi rekommenderar att du lagrar filer på Azure Blob storage. BLOB storage är permanent.

## <a name="list-and-show-clusters"></a>Listan och visa kluster
1. Logga in för[https://portal.azure.com](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** hello vänstra menyn.
3. Klicka på hello klusternamnet. Om hello klusterlista är långt kan du använda filter på hello hello överst.
4. Dubbelklicka på ett kluster från hello tooshow hello information.

    **Menyn och essentials**:

    ![Azure portal HDInsight-kluster essentials](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * toocustomize hello-menyn högerklickar du någonstans på hello-menyn och klicka sedan på **anpassa**.
   * **Inställningar för** och **alla inställningar**: Visar hello **inställningar** bladet för hello-kluster, vilket gör att du tooaccess detaljerad konfigurationsinformation för hello klustret.
   * **Instrumentpanelen**, **Klusterinstrumentpanel** och **URL: dessa är alla sätt tooaccess hello instrumentpanelen klustret, vilket är Ambari Web för Linux-baserade kluster. -**Secure Shell **: Visar hello instruktioner tooconnect toohello kluster med hjälp av SSH (Secure Shell)-anslutning.
   * **Skala klustret**: tillåter toochange hello antalet arbetarnoder för det här klustret.
   * **Ta bort**: tar bort hello klustret.
   * **Snabbstart**: Visar information som hjälper dig att komma igång med HDInsight.
   * ** Användare: Ger tooset behörigheter för *portal management* för detta kluster för andra användare på Azure-prenumerationen.

     > [!IMPORTANT]
     > Detta *endast* påverkar åtkomst och behörighet toothis klustret i hello Azure-portalen och har ingen effekt på vem som kan ansluta tooor skicka jobb toohello HDInsight-kluster.
     >
     >
   * **Taggar**: etiketter kan du tooset nyckel/värde-par toodefine en anpassad taxonomi av dina molntjänster. Du kan till exempel skapa en nyckel som heter **projekt**, och sedan använda ett värde som är gemensamma för alla tjänster som är associerad med ett visst projekt.
   * **Ambari Views**: länkar tooAmbari Web.

     > [!IMPORTANT]
     > toomanage hello tjänster som tillhandahålls av hello HDInsight-kluster, måste du använda Ambari Web eller hello Ambari REST API. Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Användning**:

     ![Azure portal HDInsight-kluster användning](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Klicka på **inställningar**.

    ![Azure portal HDInsight-kluster användning](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Egenskaper för**: Visa egenskaper för hello-klustret.
   * **Kluster-AAD-identitet**:
   * **Azure Storage-nycklar**: Visa hello standardkontot för lagring och dess nyckel. Hej lagringskonto är konfigurationen under hello klusterskapandeprocessen.
   * **Klustret inloggningen**: ändra hello klustret HTTP-användarnamn och lösenord.
   * **Externa Metastores**: Visa hello Hive och Oozie metastores. Hej metastores kan bara konfigureras när hello klustret skapas.
   * **Skala klustret**: ökning och minskning hello antalet arbetarnoder i klustret.
   * **Fjärrskrivbord**: aktivera och inaktivera åtkomst via fjärrskrivbord (RDP) och konfigurera hello RDP-användarnamnet.  hello RDP-användarnamnet måste skilja sig från hello HTTP-användarnamn.
   * **Auktoriserad partner**:

     > [!NOTE]
     > Det här är en generisk lista över tillgängliga inställningar. inte alla finnas för alla typer av klustret.
     >
     >
6. Klicka på **egenskaper**:

    Hej egenskapsavsnittet visar hello följande:

   * **Hostname**: klustrets namn.
   * **Kluster-URL: en**.
   * **Status för**: inkludera avbröts, godkända ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, fungerar, kör, fel, ta bort, tas bort, för lång tid, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued ResizeQueued, ClusterCustomization
   * **Region**: Azure-plats. En lista över Azure platser som stöds finns i hello **Region** nedrullningsbar listruta i [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Data som har skapats**.
   * **Operativsystemet**: antingen **Windows** eller **Linux**.
   * **Typen**: Hadoop, HBase, Storm, Väck.
   * **Version**. Se [HDInsight-versioner](hdinsight-component-versioning.md)
   * **Prenumerationen**: prenumerationsnamn.
   * **Prenumerations-ID**.
   * **Primär datakälla**. hello Azure Blob storage-konto som används som standard hello Hadoop-filsystem.
   * **Arbetsnoderna prisnivån**.
   * **Huvudnod prisnivån**.

## <a name="delete-clusters"></a>Ta bort kluster
Ett kluster tas inte bort hello standardkontot för lagring eller eventuella länkade lagringskonton. Du kan återskapa hello kluster med hjälp av hello samma storage-konton och hello samma metastores.

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.
3. Klicka på **ta bort** hello översta menyn och sedan hello anvisningar.

Se även [paus/stängs av kluster](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Skala kluster
hello klustret skalning funktionen kan du toochange hello antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan toore-skapa hello-kluster.

> [!NOTE]
> Endast kluster med HDInsight version 3.1.3 eller högre stöds. Du kan kontrollera hello-egenskapssidan om du är osäker på hello version av klustret.  Se [listan och visa](#list-and-show-clusters).
>
>

hello konsekvenser av att ändra hello antalet datanoder för varje typ av kluster som stöds av HDInsight:

* Hadoop

    Sömlöst kan du öka hello antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb. Nya jobb kan också skicka medan hello-åtgärd pågår. Fel i en åtgärd för skalning hanteras korrekt så att hello klustret alltid lämnas i ett fungerande tillstånd.

    När ett Hadoop-kluster skalas genom att minska hello antalet datanoder hello tjänster i hello kluster har startats om. Detta medför att alla körs och väntande jobb toofail på hello hello skalning åtgärden har slutförts. Du kan dock skicka hello jobb när hello åtgärden är klar.
* HBase

    Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs. Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen. Du kan också manuellt balansera hello regionala servrar genom att logga in hello headnode för klustret och köra hello följande kommandon från en kommandotolk:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Mer information om hur du använder hello HBase-gränssnittet finns i]
* Storm

    Sömlöst kan du lägga till eller ta bort data noder tooyour Storm-kluster medan den körs. Men när en slutförd hello skalning åtgärden måste toorebalance hello-topologi.

    Ombalansering kan utföras på två sätt:

  * Storm webbgränssnittet
  * Verktyget kommandoradsgränssnittet (CLI)

    Se toohello [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.

    hello Storm webbgränssnittet är tillgänglig på hello HDInsight-kluster:

    ![HDInsight storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**tooscale kluster**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.
3. Klicka på **inställningar** hello översta menyn och klicka sedan på **kluster**.
4. Ange **numret för arbetarnoder**. hello varierar hello antalet klusternod mellan olika Azure-prenumerationer. Du kan kontakta faktureringssupporten tooincrease hello supportgränsen.  hello kostnadsinformation visar hello ändringar du har gjort toohello antalet noder.

    ![HDInsight Hadoop HBase, Storm Spark skala](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Pausa/stängs av kluster
De flesta Hadoop-jobb är batchjobb som bara ibland kördes. Det finns stora perioder för de flesta Hadoop-kluster tid hello klustret inte används för bearbetning. Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används.
Du debiteras också för ett HDInsight-kluster, även när det inte används. Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används.

Det finns många sätt som du kan programmet hello processen:

* Användaren Azure Data Factory. Se [länkad Azure HDInsight-tjänst](../data-factory/data-factory-compute-linked-services.md) och [transformera och analysera med hjälp av Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) för HDInsight på begäran och automatisk definierad länkade tjänster.
* Använda Azure PowerShell.  Se [analysera svarta fördröjning](hdinsight-analyze-flight-delay-data.md).
* Använda Azure CLI. Se [hantera HDInsight-kluster med hjälp av Azure CLI](hdinsight-administer-use-command-line.md).
* Använda HDInsight .NET SDK. Se [skicka Hadoop-jobb](hdinsight-submit-hadoop-jobs-programmatically.md).

Hello information om priser, se [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/). toodelete ett kluster från hello-portalen finns [ta bort kluster](#delete-clusters)

## <a name="change-cluster-username"></a>Ändra kluster användarnamn
Ett HDInsight-kluster kan ha två användarkonton. hello användarkonto för HDInsight-kluster skapas under skapandeprocessen hello. Du kan också skapa ett RDP-användarkonto för att komma åt hello klustret via RDP. Se [aktivera Fjärrskrivbord](#connect-to-hdinsight-clusters-by-using-rdp).

**toochange hello HDInsight-kluster, användarnamn och lösenord**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.
3. Klicka på **inställningar** hello översta menyn och klicka sedan på **Klusterinloggning**.
4. Om **klusterinloggning** har aktiverats, måste du klicka på **inaktivera**, och klicka sedan på **aktivera** innan du kan ändra hello användarnamn och lösenord...
5. Ändra hello **klustret inloggningsnamnet** och/eller hello **klustret inloggningslösenordet**, och klicka sedan på **spara**.

    ![HDInsight ändra klustret användarnamn lösenord http-användare](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Bevilja/återkalla åtkomst
HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Som standard beviljas dessa tjänster för åtkomst. Du kan återkalla/bevilja hello åtkomst från hello Azure-portalen.

> [!NOTE]
> Genom att bevilja/återkalla hello åtkomst, återställs hello klustret användarnamn och lösenord.
>
>

**toogrant/revoke HTTP services webbåtkomst**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.
3. Klicka på **inställningar** hello översta menyn och klicka sedan på **Klusterinloggning**.
4. Om **klusterinloggning** har aktiverats, måste du klicka på **inaktivera**, och klicka sedan på **aktivera** innan du kan ändra hello användarnamn och lösenord...
5. För **klustret inloggning användarnamn** och **klustret inloggningslösenordet**, Skriv hello nytt användarnamn och lösenord (respektive) för hello-kluster.
6. Klicka på **SPARA**.

    ![HDInsight grand ta bort HTTP-tjänsten webbåtkomst](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-hello-default-storage-account"></a>Hitta hello standardkontot för lagring
Varje HDInsight-kluster har en standardkontot för lagring. Hej standardkontot för lagring och dess nycklar för ett kluster visas under **inställningar**/**egenskaper**/**Azure Lagringsnycklar**. Se [listan och visa](#list-and-show-clusters).

## <a name="find-hello-resource-group"></a>Hitta hello resursgruppen
I läget för hello Azure Resource Manager skapas varje HDInsight-kluster med en Azure-resursgrupp. hello Azure-resursgrupp som ett kluster tillhör tooappears i:

* Hej klusterlista har en **resursgruppen** kolumn.
* Klustret **väsentliga** panelen.  

Se [listan och visa](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>Öppna konsolen för HDInsight-fråga
Hej HDInsight frågan konsolen innehåller hello följande funktioner:

* **Hive Editor**: A GUI Webbgränssnitt för att skicka Hive-jobb.  Se [köra Hive-frågor med hello frågan konsolen](hdinsight-hadoop-use-hive-query-console.md).

    ![HDInsight portal hive-redigeraren](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **Jobbhistorik**: övervaka Hadoop-jobb.  

    ![HDInsight portal jobbhistorik](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Klicka på **Frågenamnet** tooshow hello information, inklusive jobbegenskaper, **Jobbfråga**, och ** Jobbutdata. Du kan också hämta både hello frågan och hello utdata tooyour arbetsstationen.
* **Filen webbläsare**: Bläddra hello standardkontot för lagring och hello länkade lagringskonton.

    ![HDInsight portal webbläsare filsökning](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    På skärmbilden hello hello  **<Account>**  anger hello-objektet är ett Azure storage-konto.  Klicka på hello konto namn toobrowse hello filer.
* **Hadoop-Användargränssnittet**.

    ![HDInsight Hadoop UI-portalen](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Från **Hadoop UI*, kan du bläddra bland filer och loggarna.
* **Yarn UI**.

    ![HDInsight portal YARN-Användargränssnittet](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Köra Hive-frågor
tooran Hive-jobb från hello-portalen klickar du på **Hive-redigeraren** i hello HDInsight fråga-konsolen. Se [HDInsight fråga konsolen](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Övervaka jobb
toomonitor jobb från hello-portalen klickar du på **jobbhistorik** i hello HDInsight fråga-konsolen. Se [HDInsight fråga konsolen](#open-hdinsight-query-console).

## <a name="browse-files"></a>Bläddra bland filer
toobrowse filer som lagras i hello standardkontot för lagring och hello länkade lagringskonton, klickar du på **Filhanteraren** i hello HDInsight fråga-konsolen. Se [HDInsight fråga konsolen](#open-hdinsight-query-console).

Du kan också använda hello **Bläddra hello filsystemet** utility från hello **Hadoop UI** i hello HDInsight-konsolen.  Se [HDInsight fråga konsolen](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Övervaka kluster användning
Hej **användning** avsnitt i hello HDInsight-klusterbladet visar information om hello antal kärnor tillgängliga tooyour prenumerationen för användning med HDInsight, samt hello antal kärnor som allokerats toothis kluster och hur de tilldelat för hello noder i klustret. Se [listan och visa](#list-and-show-clusters).

> [!IMPORTANT]
> toomonitor hello tjänster som tillhandahålls av hello HDInsight-kluster, måste du använda Ambari Web eller hello Ambari REST API. Mer information om hur du använder Ambari finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Öppna Användargränssnittet för Hadoop
toomonitor hello klustret, bläddra hello filsystemet och kontrollera loggarna, klickar du på **Hadoop UI** i hello HDInsight fråga-konsolen. Se [HDInsight fråga konsolen](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Öppna Yarn UI
toouse Yarn-användargränssnittet, klickar du på **Yarn-Användargränssnittet** i hello HDInsight fråga-konsolen. Se [HDInsight fråga konsolen](#open-hdinsight-query-console).

## <a name="connect-tooclusters-using-rdp"></a>Ansluta tooclusters med RDP
hello autentiseringsuppgifter för hello-kluster som du angav vid starten ge åtkomst toohello tjänster på hello kluster, men inte toohello själva klustret via fjärrskrivbord. Du kan aktivera åtkomst via Fjärrskrivbord när du konfigurerar ett kluster eller efter ett kluster har etablerats. Hello instruktioner om hur du aktiverar Fjärrskrivbord när skapas finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).

**tooenable fjärrskrivbord**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.
3. Klicka på **inställningar** hello översta menyn och klicka sedan på **fjärrskrivbord**.
4. Ange **upphör att gälla på**, **användarnamnet för fjärrskrivbord** och **Remote Desktop lösenord**, och klicka sedan på **aktivera**.

    ![HDInsight aktivera inaktivera Konfigurera fjärrskrivbord](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    hello standardvärden för upphör att gälla på är en vecka.

   > [!NOTE]
   > Du kan också använda hello HDInsight .NET SDK tooenable fjärrskrivbord på ett kluster. Använd hello **EnableRdp** metod på hello HDInsight klientobjekt i hello följande sätt: **klienten. EnableRdp (klusternamn, plats, ”rdpuser”, ”rdppassword”, DateTime.Now.AddDays(6))**. På liknande sätt toodisable fjärrskrivbord på hello klustret kan du använda **klienten. DisableRdp (klusternamn, platsen)**. Mer information om dessa metoder finns [HDInsight .NET SDK referens](http://go.microsoft.com/fwlink/?LinkId=529017). Detta gäller endast för HDInsight-kluster som körs på Windows.
   >
   >

**tooconnect tooa kluster med hjälp av RDP**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.
3. Klicka på **inställningar** hello översta menyn och klicka sedan på **fjärrskrivbord**.
4. Klicka på **Anslut** och följer instruktionerna för hello. Om Connect inaktivera, måste du aktivera den först. Kontrollera med hello fjärrskrivbord användarens användarnamn och lösenord.  Du kan inte använda hello klustret användarens autentiseringsuppgifter.

## <a name="open-hadoop-command-line"></a>Öppna kommandoraden för Hadoop
tooconnect toohello kluster med hjälp av fjärrskrivbord och Använd hello Hadoop-kommandoraden, måste du först ha aktiverat Fjärrskrivbord åtkomst toohello klustret enligt beskrivningen i föregående avsnitt i hello.

**tooopen en kommandorad för Hadoop**

1. Ansluta toohello kluster med hjälp av fjärrskrivbord.
2. Dubbelklicka på skrivbordet hello **Hadoop kommandoraden**.

    ![HDI. HadoopCommandLine][image-hadoopcommandline]

    Mer information om Hadoop-kommandon finns [Hadoop kommandon referens](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

I föregående skärmbilden hello har hello mappnamn hello Hadoop versionsnummer inbäddade. hello versionsnumret kan ändrade utifrån hello version av hello Hadoop-komponenter installeras på hello-kluster. Du kan använda Hadoop-miljön variabler toorefer toothose mappar. Exempel:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toocreate ett HDInsight-kluster med hjälp av hello Portal och hur tooopen hello Hadoop-kommandoradsverktyget. toolearn se fler hello följande artiklar:

* [Administrera HDInsight med hjälp av Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Administrera HDInsight med hjälp av Azure CLI](hdinsight-administer-use-command-line.md)
* [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)
* [Skicka Hadoop-jobb via programmering](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Komma igång med Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Vilken version av Hadoop finns i Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop-kommandorad"
