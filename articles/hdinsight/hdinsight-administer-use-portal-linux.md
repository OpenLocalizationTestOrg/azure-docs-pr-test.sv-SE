---
title: "aaaManage Hadoop-kluster i HDInsight med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur toocreate och hantera HDInsight-kluster med hello Azure-portalen."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: c242d43d4ccea7cf1e7be19c3f3d7ed3c4f50918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a>Hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Med hjälp av hello [Azure-portalen][azure-portal], kan du hantera Hadoop-kluster i Azure HDInsight. Använd hello flikväljaren för information om hur du hanterar Hadoop-kluster i HDInsight med andra verktyg.

**Förutsättningar**

Innan du börjar den här artikeln, måste du ha hello följande objekt:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-hello-portal"></a>Öppna hello-portalen
1. Logga in för[https://portal.azure.com](https://portal.azure.com).
2. När du har öppnat hello-portalen kan du:

   * Klicka på **ny** från hello vänstra menyn toocreate ett nytt kluster:

       ![ny knapp för HDInsight-kluster](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Klicka på **HDInsight-kluster** från hello vänstra menyn toolist hello befintliga kluster

       ![Azure portal HDInsight-kluster-knappen](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Om du inte ser HDInsight-kluster, klickar du på **fler tjänster** på hello längst ned på hello listan och klicka sedan på **HDInsight-kluster** under hello **Intelligence + analys** avsnittet.


## <a name="create-clusters"></a>Skapa kluster
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight fungerar med en bred Hadoop-komponenter. Hello lista hello-komponenter som har verifierats, och som stöds finns i [vilken version av Hadoop finns i Azure HDInsight](hdinsight-component-versioning.md). Hello allmänna klustret skapas information finns i [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Åtkomstkontrollkrav

När du skapar ett HDInsight-kluster måste du ange en Azure-prenumeration. Det här klustret kan skapas i en ny Azure resursgrupp eller en befintlig resursgrupp. Du kan använda följande steg tooverify hello din behörighet för att skapa HDInsight-kluster:

- toouse en befintlig resursgrupp.

    1. Logga in toohello [Azure-portalen](https://portal.azure.com).
    2. Klicka på **resursgrupper** från hello vänstra menyn toolist hello resursgrupper.
    3. Klicka på hello resursgrupp toouse för att skapa ditt HDInsight-kluster.
    4. Klicka på **åtkomstkontroll (IAM)**, och kontrollera att du (eller en grupp som du tillhör) har minst hello deltagare åtkomst toohello resursgruppen.

- toocreate en ny resursgrupp

    1. Logga in toohello [Azure-portalen](https://portal.azure.com).
    2. Klicka på **prenumeration** hello vänstra menyn. Den har en gul nyckelikonen. Du bör se en lista över prenumerationer.
    3. Klicka på hello-prenumeration som du använder toocreate kluster. 
    4. Klicka på **min behörighet**.  Det visar dina [rollen](../active-directory/role-based-access-control-what-is.md#built-in-roles) på hello prenumeration. Du behöver minst deltagare åtkomst toocreate HDInsight-kluster.

Om du får hello NoRegisteredProviderFound fel eller hello MissingSubscriptionRegistration läser [felsöka vanliga Azure-distribution med Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Listan och visa kluster
1. Logga in för[https://portal.azure.com](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** från hello vänstra menyn toolist hello befintliga kluster.
3. Klicka på hello klusternamnet. Om hello klusterlista är långt kan du använda filter på hello hello överst.
4. Klicka på ett kluster från sidan Översikt över hello toosee hello:

    ![Azure portal HDInsight-kluster essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * **Instrumentpanelen**: öppnas hello instrumentpanelen klustret, vilket är Ambari Web för Linux-baserade kluster.
    * **Secure Shell**: Visar hello instruktioner tooconnect toohello kluster med hjälp av SSH (Secure Shell)-anslutning.
    * **Skala klustret**: tillåter toochange hello antalet arbetarnoder för det här klustret.
    * **Ta bort**: tar bort hello klustret.
    * **Aktivitetsloggar**: visa och fråga aktivitetsloggar.
    * **Åtkomstkontroll (IAM)**: Använd rolltilldelningar.  Se [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md).
    * **Taggar**: gör att du tooset nyckel/värde-par toodefine en anpassad taxonomi av dina molntjänster. Du kan till exempel skapa en nyckel som heter **projekt**, och sedan använda ett värde som är gemensamma för alla tjänster som är associerad med ett visst projekt.
    * **Diagnostisera och lösa problem**: visa felsökningsinformation.
    * **Låser**: Lägg till Lås tooprevent hello klustret som ändras eller tas bort.
    * **Automatiseringsskriptet**: visa och exportera hello Azure Resource Manager-mall för hello-kluster. Du kan för närvarande bara exportera hello beroende Azure storage-konto. Se [skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Azure Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Snabbstart**: Visar information som hjälper dig att komma igång med HDInsight.
    * **Verktyg för HDInsight**: hjälpinformation för HDInsight relaterade verktyg.
    * **Klustret inloggningen**: Visa hello inloggningsinformation för klustret.
    * **Prenumerationen Core användning**: Visa hello Använd och tillgänglig kärnor för prenumerationen.
    * **Skala klustret**: ökning och minskning hello antalet arbetarnoder i klustret. Se[skala kluster](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Secure Shell**: Visar hello instruktioner tooconnect toohello kluster med hjälp av SSH (Secure Shell)-anslutning. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).
    * **HDInsight-partnern**: Lägg till/ta bort hello aktuell HDInsight-Partner.
    * **Externa Metastores**: Visa hello Hive och Oozie metastores. Hej metastores kan bara konfigureras när hello klustret skapas. Se [använda Hive/Oozie metastore](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Script åtgärder**: köra Bash-skript på hello klustret. Se [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).
    * **Program**: Lägg till/ta bort HDInsight-program.  Se [installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md).
    * **Egenskaper för**: Visa egenskaper för hello-klustret.
    * **Storage-konton**: Visa hello storage-konton och hello nycklar. hello storage-konton har konfigurerats under hello klusterskapandeprocessen.
    * **Kluster-AAD-identitet**:
    * **Ny supportbegäran**: tillåter toocreate ett supportärende Microsoft support.
    
6. Klicka på **egenskaper**:

    hello egenskaper är:

   * **Hostname**: klustrets namn.
   * **Kluster-URL: en**. hello-URL för hello Ambari-webbgränssnittet.
   * **Status för**: inkludera avbröts, godkända ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, fungerar, kör, fel, ta bort, tas bort, för lång tid, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued ResizeQueued, ClusterCustomization
   * **Region**: Azure-plats. En lista över Azure platser som stöds finns i hello **Region** nedrullningsbar listruta i [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Skapandedatum**.
   * **Operativsystemet**: antingen **Windows** eller **Linux**.
   * **Typen**: Hadoop, HBase, Storm, Väck.
   * **Version**. Se [HDInsight-versioner](hdinsight-component-versioning.md)
   * **Prenumerationen**: prenumerationsnamn.
   * **Standarddatakälla**: hello standardfilsystem för klustret.
   * **Worker noder storlek**.
   * **Gå nodstorlek**.

## <a name="delete-clusters"></a>Ta bort kluster
Tar bort ett kluster tar inte bort hello standardkontot för lagring eller eventuella länkade lagringskonton. Du kan återskapa hello kluster med hjälp av hello samma storage-konton och hello samma metastores. Det är rekommenderat toouse en ny standardbehållaren när du återskapar hello klustret.

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **HDInsight-kluster** hello vänstra menyn. Om du inte ser **HDInsight-kluster**, klickar du på **fler tjänster** första.
3. Klicka på hello kluster som du vill toodelete.
4. Klicka på **ta bort** hello översta menyn och sedan hello anvisningar.

Se även [paus/stängs av kluster](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Lägga till fler lagringskonton

Du kan lägga till ytterligare Azure Storage-konton och Azure Data Lake Store-konton när ett kluster skapas. Mer information finns i [lägga till ytterligare lagringsutrymme konton tooHDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Skala kluster
hello klustret skalning funktionen kan du toochange hello antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan toore-skapa hello-kluster.

> [!NOTE]
> Endast kluster med HDInsight version 3.1.3 eller högre stöds. Du kan kontrollera hello-egenskapssidan om du är osäker på hello version av klustret.  Se [listan och visa](#list-and-show-clusters).
>
>

hello konsekvenser av att ändra hello antalet datanoder för varje typ av kluster som stöds av HDInsight:

* Hadoop

    Sömlöst kan du öka hello antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb. Nya jobb kan också skicka medan hello-åtgärd pågår. Fel i en åtgärd för skalning hanteras korrekt så att hello klustret alltid lämnas i ett fungerande tillstånd.

    När ett Hadoop-kluster skalas genom att minska hello antalet datanoder hello tjänster i hello kluster har startats om. Detta medför alla körs och väntande jobb toofail på hello hello skalning åtgärden har slutförts. Du kan dock skicka hello jobb när hello åtgärden är klar.
* HBase

    Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs. Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen. Du kan också manuellt balansera hello regionala servrar genom att logga in toohello headnode för klustret och köra hello följande kommandon från en kommandotolk:

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

    ![HDInsight Storm skala balansera](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**tooscale kluster**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **HDInsight-kluster** hello vänstra menyn.
3. Klicka på hello-kluster som du vill ha tooscale.
3. Klicka på **skala klustret**.
4. Ange **numret för arbetarnoder**. hello varierar hello antalet klusternoder mellan olika Azure-prenumerationer. Du kan kontakta faktureringssupporten tooincrease hello supportgränsen.  hello kostnadsinformation visar hello ändringar du har gjort toohello antalet noder.

    ![HDInsight hadoop hbase storm spark skala](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Pausa/stängs av kluster

De flesta Hadoop-jobb är batchjobb som endast körs ibland. Det finns stora perioder för de flesta Hadoop-kluster tid hello klustret inte används för bearbetning. Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används.
Du debiteras också för ett HDInsight-kluster, även när det inte används. Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används.

Det finns många sätt som du kan programmet hello processen:

* Användaren Azure Data Factory. Se [skapa på begäran Linux-baserade Hadoop-kluster i HDInsight med hjälp av Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) länkade tjänster för att skapa HDInsight på begäran.
* Använda Azure PowerShell.  Se [analysera svarta fördröjning](hdinsight-analyze-flight-delay-data.md).
* Använda Azure CLI. Se [hantera HDInsight-kluster med hjälp av Azure CLI](hdinsight-administer-use-command-line.md).
* Använda HDInsight .NET SDK. Se [skicka Hadoop-jobb](hdinsight-submit-hadoop-jobs-programmatically.md).

Hello information om priser, se [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/). toodelete ett kluster från hello-portalen finns [ta bort kluster](#delete-clusters)


## <a name="upgrade-clusters"></a>Uppgradera kluster

Se [uppgradera HDInsight-kluster tooa nyare version](./hdinsight-upgrade-cluster.md).

## <a name="change-passwords"></a>Ändra lösenord
Ett HDInsight-kluster kan ha två användarkonton. Hej användarkonto för HDInsight-klustret (kallas även HTTP-användarkontot) och hello SSH-användarkontot skapas under skapandeprocessen hello. Du kan använda hello Ambari web UI toochange hello klustret användaren Kontoanvändarnamn och lösenord och skript åtgärder toochange hello SSH-användarkontot

### <a name="change-hello-cluster-user-password"></a>Ändra hello klustret användarens lösenord
Du kan använda hello Ambari-Webbgränssnittet toochange hello klustret användarens lösenord. toolog i tooAmbari måste du använda hello befintliga kluster användarnamn och lösenord.

> [!NOTE]
> Ändra hello kluster (admin) användarlösenord kan orsaka skript åtgärder har körts för det här klustret toofail. Om du har några beständiga skriptåtgärder riktade arbetarnoder misslyckas skripten när du lägger till noder toohello klustret via storleksändring åtgärder. Mer information om skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Inloggning toohello Ambari-Webbgränssnittet med hello autentiseringsuppgifter för HDInsight-kluster. hello Standardanvändarnamnet är **admin**. hello URL: en är **https://&lt;HDInsight-klustrets namn > azurehdinsight.net**.
2. Klicka på **Admin** hello översta menyn och klicka sedan på ”Hantera Ambari”.
3. Hello vänstra menyn klickar du på **användare**.
4. Klicka på **Admin**.
5. Klicka på **ändra lösenord**.

Ambari och ändrar hello lösenord på alla noder i klustret hello.

### <a name="change-hello-ssh-user-password"></a>Ändra hello SSH användarens lösenord
1. Med hjälp av en textredigerare, spara hello följande text som en fil med namnet **changepassword.sh**.

   > [!IMPORTANT]
   > Du måste använda ett redigeringsprogram som använder LF som hello rad avslutas. Om hello Redigerare använder CRLF, fungerar inte hello skript.
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Överför hello tooa lagringsplatsen för migreringsfilen som kan nås från HDInsight med hjälp av en HTTP- eller HTTPS-adress. Till exempel lagra en gemensam fil, t.ex OneDrive eller Azure Blob storage. Spara hello URI (HTTP eller HTTPS-adress) toohello filen när den här URI behövs i hello nästa steg.
3. Hello Azure-portalen, klicka på **HDInsight-kluster**.
4. Klicka på ditt HDInsight-kluster.
4. Klicka på **skript åtgärder**.
4. Från hello **skriptåtgärder** bladet väljer **skicka nya**. När hello **skicka skriptåtgärden** bladet visas, ange hello följande information:

   | Fält | Värde |
   | --- | --- |
   | Namn |Ändra ssh lösenord |
   | Bash-skript URI |hello URI toohello changepassword.sh fil |
   | Noder (Head, Worker, Nimbus, chef, Zookeeper osv.) |✓ för alla nodtyper som anges |
   | Parametrar |Ange hello SSH-användarnamn och hello nytt lösenord. Det bör finnas ett blanksteg mellan hello-användarnamn och lösenord för hello. |
   | Spara den här skriptåtgärden... |Lämna det här fältet är avmarkerat. |
5. Välj **skapa** tooapply hello skript. När hello skriptet har slutförts är kan tooconnect toohello kluster med SSH med hello nytt lösenord.

## <a name="grantrevoke-access"></a>Bevilja/återkalla åtkomst
HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Som standard beviljas dessa tjänster för åtkomst. Du kan återkalla/bevilja hello med [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) och [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-hello-subscription-id"></a>Hitta hello prenumerations-ID

**toofind azureprenumerationen ID: N**

1. Logga in toohello [Portal][azure-portal].
2. Klicka på **prenumerationer**. Varje prenumeration har ett namn och ett ID.

Varje kluster är bundet tooan Azure-prenumeration. Hej prenumeration ID visas i hello klustret **väsentliga** panelen. Se [listan och visa](#list-and-show-clusters).

## <a name="find-hello-resource-group"></a>Hitta hello resursgruppen
I läget för hello Azure Resource Manager skapas varje HDInsight-kluster med en Azure Resource Manager-grupp. hello Resource Manager-grupp som en klustret tillhör tooappears i:

* Hej klusterlista har en **resursgruppen** kolumn.
* Klustret **väsentliga** panelen.  

Se [listan och visa](#list-and-show-clusters).

## <a name="find-hello-default-storage-account"></a>Hitta hello standardkontot för lagring
Varje HDInsight-kluster har en standardkontot för lagring. Hej standardkontot för lagring och dess nycklar för ett kluster visas under **Lagringskonton**. Se [listan och visa](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Köra Hive-frågor
Du kan inte köra Hive-jobb direkt från hello Azure-portalen, men du kan använda hello Hive-vy på Ambari-Webbgränssnittet.

**toorun Hive-frågor med Ambari Hive-vy**

1. Inloggning toohello Ambari-Webbgränssnittet med hello autentiseringsuppgifter för HDInsight-kluster. hello Standardanvändarnamnet är **admin**. hello URL: en är **https://&lt;HDInsight-klustrets namn > azurehdinsight.net**.
2. Öppna Hive-vy som visas i följande skärmbild hello:  

    ![HDInsight hive-vyn](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Klicka på **frågan** hello översta menyn.
4. Ange en Hive-fråga i **frågeredigeraren**, och klicka sedan på **kör**.

## <a name="monitor-jobs"></a>Övervaka jobb
Se [hantera HDInsight-kluster med hjälp av hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Bläddra bland filer
Du kan bläddra hello innehållet i hello standardbehållaren med hello Azure-portalen.

1. Logga in för[https://portal.azure.com](https://portal.azure.com).
2. Klicka på **HDInsight-kluster** från hello vänstra menyn toolist hello befintliga kluster.
3. Klicka på hello klusternamnet. Om hello klusterlista är långt kan du använda filter på hello hello överst.
4. Klicka på **Lagringskonton** hello klustret vänstra menyn.
5. Klicka på ett lagringskonto.
7. Klicka på hello **Blobbar** panelen.
8. Klicka på hello standardnamnet för behållaren.

## <a name="monitor-cluster-usage"></a>Övervaka kluster användning
Hej **användning** avsnitt i hello HDInsight-klusterbladet visar information om hello antal kärnor tillgängliga tooyour prenumerationen för användning med HDInsight, samt hello antal kärnor som allokerats toothis kluster och hur de tilldelat för hello noder i klustret. Se [listan och visa](#list-and-show-clusters).

> [!IMPORTANT]
> toomonitor hello tjänster som tillhandahålls av hello HDInsight-kluster, måste du använda Ambari Web eller hello Ambari REST API. Mer information om hur du använder Ambari finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="connect-tooa-cluster"></a>Ansluta tooa kluster

* [Använda Hive med HDInsight](hdinsight-hadoop-use-hive-ambari-view.md)
* [Använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig vissa grundläggande administative funktioner. toolearn se fler hello följande artiklar:

* [Administrera HDInsight med hjälp av Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Administrera HDInsight med hjälp av Azure CLI](hdinsight-administer-use-command-line.md)
* [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)
* [Använda Hive i HDInsight](hdinsight-use-hive.md)
* [Använda Pig i HDInsight](hdinsight-use-pig.md)
* [Använda Sqoop i HDInsight](hdinsight-use-sqoop.md)
* [Komma igång med Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Vilken version av Hadoop finns i Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-kommandorad"
