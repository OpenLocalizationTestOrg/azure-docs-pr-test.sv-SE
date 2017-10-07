---
title: "principer för aaaConfigure Hive i HDInsight med domänanslutna - Azure | Microsoft Docs"
description: "Läs mer ..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 56f2bf9d872abc5f772b886fcf91c2e2422092f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a>Konfigurera Hive-principer i domänanslutna HDInsight (förhandsversion)
Lär dig hur tooconfigure Apache Ranger principer för Hive. I den här artikeln får skapa du två Ranger principer toorestrict åtkomst toohello hivesampletable. Hej hivesampletable medföljer HDInsight-kluster. När du har konfigurerat hello principer kan använda du Excel och ODBC-drivrutinen tooconnect tooHive tabeller i HDInsight.

## <a name="prerequisites"></a>Krav
* Ett domänanslutet HDInsight-kluster. Se [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).
* En arbetsstation med Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, fristående Excel 2013 eller Office 2010 Professional Plus.

## <a name="connect-tooapache-ranger-admin-ui"></a>Ansluta tooApache Ranger Admin UI
**tooconnect tooRanger Admin UI**

1. Ansluta tooRanger Admin UI från en webbläsare. hello URL: en är https://&lt;klusternamn >.azurehdinsight.net/Ranger/.

   > [!NOTE]
   > Ranger använder andra autentiseringsuppgifter än Hadoop-kluster. tooprevent webbläsare med den cachelagrade Hadoop autentiseringsuppgifter använder nya InPrivate-webbläsaren fönstret tooconnect toohello Ranger Admin-Användargränssnittet.
   >
   >
2. Logga in med hello klustret administratör domän, användarnamn och lösenord:

    ![Startsida för HDInsight domänanslutet Ranger](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    För närvarande fungerar Ranger bara med Yarn och Hive.

## <a name="create-domain-users"></a>Skapa domänanvändare
I [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) har du skapat hiveruser1 och hiveuser2. Du använder hello två användarkonto i den här kursen.

## <a name="create-ranger-policies"></a>Skapa Ranger-principer
I det här avsnittet skapar du två Ranger-principer för att komma åt hivesampletable. Du kan ge select-behörighet för olika uppsättningar med kolumner. Båda användarna skapades i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).  I nästa avsnitt om hello, ska du testa hello två principer i Excel.

**toocreate Ranger principer**

1. Öppna Ranger Admin-gränssnittet. Se [ansluta tooApache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).
2. Klicka på **&lt;Klusternamn>_hive** under **Hive**. Du bör se två förkonfigurerade principer.
3. Klicka på **Lägg till ny princip**, och ange sedan hello följande värden:

   * Principnamn: read-hivesampletable-all
   * Hive-database: standard
   * tabell: hivesampletable
   * Hive-kolumn: *
   * Välj användare: hiveuser1
   * Behörigheter: select

     ![Konfigurera Ranger Hive-princip för domänansluten HDInsight](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

     > [!NOTE]
     > Om en domänanvändare inte har fyllts i i Välj användare, vänta en stund för Ranger toosync med AAD.
     >
     >
4. Klicka på **Lägg till** toosave hello princip.
5. Upprepa hello två sista stegen toocreate en annan princip med hello följande egenskaper:

   * Principnamn: read-hivesampletable-devicemake
   * Hive-database: standard
   * tabell: hivesampletable
   * Hive-kolumn: clientid, devicemake
   * Välj användare: hiveuser2
   * Behörigheter: select

## <a name="create-hive-odbc-data-source"></a>Skapa Hive ODBC-datakälla
hello anvisningar finns i [skapa Hive ODBC-datakällan](hdinsight-connect-excel-hive-odbc-driver.md).  

    Egenskap|Beskrivning
    ---|---
    Namn på datakälla|Ge en namnet tooyour-datakälla
    Värd|Ange &lt;HDInsightClusterName>.azurehdinsight.net. Till exempel myHDICluster.azurehdinsight.net
    Port|Använd <strong>443</strong>. (Den här porten har ändrats från 563 too443.)
    Databas|Använd <strong>Standard</strong>.
    Hive-servertyp|Välj <strong>Hive Server 2</strong>
    Mekanism|Välj <strong>Azure HDInsight-tjänst</strong>
    HTTP-sökväg|Lämna tomt.
    Användarnamn|Ange hiveuser1@contoso158.onmicrosoft.com. Uppdatera hello domännamnet om det är olika.
    Lösenord|Ange hello lösenord för hiveuser1.
    </table>

Se till att tooclick **Test** innan du sparar hello-datakälla.

## <a name="import-data-into-excel-from-hdinsight"></a>Importera data till Excel från HDInsight
Du har konfigurerat två principer under hello senaste.  hiveuser1 har hello markerar behörighet på alla hello kolumner och hiveuser2 har hello markerar behörighet på två kolumner. I det här avsnittet personifiera hello två användare tooimport data till Excel.

1. Öppna en ny eller befintlig arbetsbok i Excel.
2. Från hello **Data** klickar du på **från andra datakällor**, och klicka sedan på **från guiden** toolaunch hello **Dataanslutningsguiden**.

    ![Öppna guiden Dataanslutning][img-hdi-simbahiveodbc.excel.dataconnection]
3. Välj **ODBC DSN** som hello datakällan och klicka sedan på **nästa**.
4. ODBC-datakällor, Välj hello data datakällans namn som du skapade i föregående steg i hello, och klicka sedan på **nästa**.
5. Anger hello lösenordet för hello-kluster i hello guiden igen och klicka sedan på **OK**. Vänta tills hello **Välj databas och tabell** dialogrutan tooopen. Det kan ta några sekunder.
6. Välj **hivesampletable** och klicka sedan på **Nästa**.
7. Klicka på **Slutför**.
8. I hello **importera Data** dialogrutan kan du ändra eller ange hello-fråga. toodo Klicka på **egenskaper**. Det kan ta några sekunder.
9. Klicka på hello **Definition** fliken hello Kommandotext är:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Hello Ranger principer som du har definierat, har hiveuser1 select-behörighet på alla hello-kolumner.  Så den här frågan fungerar med autentiseringsuppgifterna för hiveuser1, men frågan fungerar inte med autentiseringsuppgifterna för hiveuser2.

   ![Anslutningsegenskaper][img-hdi-simbahiveodbc-excel-connectionproperties]
10. Klicka på **OK** tooclose hello anslutningsegenskaper.
11. Klicka på **OK** tooclose hello **importera Data** dialogrutan.  
12. Ange hello lösenordet för hiveuser1 igen och klicka sedan på **OK**. Det tar några sekunder innan data hämtar importerade tooExcel. När det är klart bör du se 11 kolumner med data.

tootest hello andra princip (Läs-hivesampletable-devicemake) som du skapade i hello sista avsnittet

1. Lägg till ett nytt kalkylblad i Excel.
2. Följ hello sista proceduren tooimport hello data.  hello endast ändringar du gör är toouse hiveuser2 autentiseringsuppgifter i stället för hiveuser1's. Detta kommer att misslyckas eftersom hiveuser2 har endast behörighet toosee två kolumner. Du bör få hello följande fel:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Följ hello samma procedur tooimport data. Den här tiden kan använda hiveuser2's autentiseringsuppgifter och även modifiera hello select-instruktion från:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    till:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    När det är klart bör du se två kolumner med importerade data.

## <a name="next-steps"></a>Nästa steg
* Om du vill konfigurera ett domänanslutet HDInsight-kluster kan du läsa i [Konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md).
* Om du vill hantera ett domänanslutet HDInsight-kluster kan du läsa i [Hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).
* För att köra Hive-frågor med SSH på domänanslutna HDInsight-kluster, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Ansluta Hive med Hive JDBC, finns [ansluta tooHive på Azure HDInsight med hello Hive JDBC-drivrutinen](hdinsight-connect-hive-jdbc-driver.md)
* Ansluta Excel tooHadoop med hjälp av Hive ODBC finns [ansluta Excel tooHadoop med hello Microsoft Hive ODBC-enhet](hdinsight-connect-excel-hive-odbc-driver.md)
* Ansluta Excel tooHadoop med Power Query finns [ansluta Excel tooHadoop med Power Query](hdinsight-connect-excel-power-query.md)
