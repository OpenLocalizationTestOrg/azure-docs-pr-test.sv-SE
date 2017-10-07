---
title: "aaaData Lake-verktyg för Visual Studio med Hortonworks Sandbox - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Data Lake-verktyg för Visual Studio med hello Hortonworks sandbox körs på en lokal virtuell dator. Med dessa verktyg kan du skapa och köra Hive och Pig-jobb på hello sandbox och visa jobbutdata och historik."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 7a3406b28d87d953e9b5be387faa86268f47b935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-data-lake-tools-for-visual-studio-with-hello-hortonworks-sandbox"></a>Använda hello Azure Data Lake-verktyg för Visual Studio med hello Hortonworks Sandbox

Azure Data Lake innehåller verktyg för att arbeta med allmänt Hadoop-kluster. Det här dokumentet innehåller hello steg behövs toouse hello Data Lake-verktyg med hello Hortonworks Sandbox körs i en lokal virtuell dator.

Om du använder hello Hortonworks Sandbox kan du toowork med Hadoop lokalt på din utvecklingsmiljö. När du har utvecklat en lösning och vill toodeploy den i skala, kan du flytta tooan HDInsight-kluster.

## <a name="prerequisites"></a>Krav

* Hej Hortonworks Sandbox, körs i en virtuell dator på din utvecklingsmiljö. Det här dokumentet har skrivits och testas med hello sandbox körs i Oracle VirtualBox. Information om hur du konfigurerar hello sandbox finns hello [Kom igång med hello Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md) dokumentet.

* Visual Studio 2013, Visual Studio 2015 eller Visual Studio 2017 (någon utgåva).

* Hej [Azure SDK för .NET](https://azure.microsoft.com/downloads/) 2.7.1 eller senare.

* Hej [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-hello-sandbox"></a>Konfigurera lösenord för hello begränsat läge

Se till att hello Hortonworks Sandbox körs. Följ stegen hello i hello [komma igång i hello Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentet. De här stegen konfigurerar hello lösenordet för hello SSH `root` konto och hello Ambari `admin` konto. Dessa lösenord används när du ansluter toohello sandbox från Visual Studio.

## <a name="connect-hello-tools-toohello-sandbox"></a>Ansluta hello verktyg toohello sandbox

1. Öppna Visual Studio, markera **visa**, och välj sedan **Server Explorer**.

2. Från **Server Explorer**, högerklicka på hello **HDInsight** posten och välj sedan **ansluta tooHDInsight emulatorn**.

    ![Skärmbild av Server Explorer, med Connect tooHDInsight emulatorn markerat](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. Från hello **ansluta tooHDInsight emulatorn** dialogrutan Ange hello-lösenord som du konfigurerade för Ambari.

    ![Skärmbild av dialogrutan med lösenordet i textrutan markerat](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Välj **nästa** toocontinue.

4. Använd hello **lösenord** fältet tooenter hello lösenordet som du konfigurerade för hello `root` konto. Lämna hello andra fälten i hello standardvärdet.

    ![Skärmbild av dialogrutan med lösenordet i textrutan markerat](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Välj **nästa** toocontinue.

5. Vänta på verifiering av hello services toofinish. I vissa fall verifieringen misslyckas och du uppmanas tooupdate hello konfiguration. Om valideringen misslyckas, Välj **uppdatering**, och vänta tills hello konfiguration och verifiering för hello tjänsten toofinish.

    ![Skärmbild av dialogrutan med knappen Uppdatera markerat](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > hello uppdateringsprocessen använder Ambari toomodify hello Hortonworks Sandbox configuration toowhat förväntas av hello Data Lake-verktyg för Visual Studio.

6. När verifieringen är klar väljer du **Slutför** toocomplete konfiguration.
    ![Skärmbild av dialogrutan med Slutför markerat](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > Beroende hello hastighet utvecklingsmiljö och hello mängden minne som allokerats toohello virtuell dator, kan det ta flera minuter tooconfigure och verifiera hello tjänster.

När du har följt de här stegen, nu har du en **lokala HDInsight-kluster** posten i Server Explorer under hello **HDInsight** avsnitt.

## <a name="write-a-hive-query"></a>Skriv en Hive-fråga

Strukturen innehåller ett SQL-liknande frågespråk (HiveQL) för att arbeta med strukturerade data. Använd hello följande steg toolearn hur toorun på begäran frågor mot hello lokala klustret.

1. I **Server Explorer**Högerklicka hello posten för lokala hello-klustret som du lagt till tidigare och välj sedan **skriva en Hive-fråga**.

    ![Skärmbild av Server Explorer med Skriv en Hive-fråga som markerats](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Ett nytt frågefönster visas. Här kan du snabbt skriva och skicka en fråga toohello lokala klustret.

2. Ange följande kommando hello i hello nytt frågefönster:

        select count(*) from sample_08;

    toorun hello fråga, Välj **skicka** hello överst i fönstret hello. Lämna hello andra värden (**Batch** och namn på servern) till hello standardvärden.

    ![Skärmbild av frågefönstret med hello Skicka-knapp markerat](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Du kan också använda hello listrutan bredvid för**skicka** tooselect **Avancerat**. Avancerade alternativ kan du tooprovide ytterligare alternativ när du skickar hello jobb.

    ![Dialogrutan Skicka skript skärmbild](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. När du har skickat hello frågan visas hello jobbstatus. hello jobbstatus visar information om hello jobb som bearbetas av Hadoop. **Jobbets status** ger hello hello jobbets status. hello tillstånd uppdateras regelbundet eller du kan använda hello uppdatera ikonen toorefresh hello tillstånd manuellt.

    ![Skärmbild av jobbet dialogrutan vy med jobbstatus markerat](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Efter hello **jobbstatus** ändras för**avslutad**, dirigeras acykliska diagram (DAG) visas. Det här diagrammet beskrivs hello körning av sökväg som fastställts av Tez vid bearbetning av hello Hive-fråga. Tez är hello standard Körningsmotor för Hive i hello lokala klustret.

    > [!NOTE]
    > Tez är också hello standard när du använder Linux-baserade HDInsight-kluster. Det är inte hello standard på Windows-baserade HDInsight. toouse den där, måste du lägga till hello rad `set hive.execution.engine = tez;` toohello början av Hive-fråga.

    Använd hello **Jobbutdata** länka tooview hello utdata. I det här fallet är det 823 hello antalet rader i hello sample_08 tabell. Du kan visa diagnostikinformation om hello jobb med hjälp av hello **Jobblogg** och **hämta YARN-logg** länkar.

4. Du kan också köra Hive-jobb interaktivt genom att ändra hello **Batch** fältet för**interaktiv**. Välj sedan **kör**.

    ![Skärmbild av interaktiva och kör knappar markerat](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    En interaktiv fråga dataströmmar hello utdataloggen genereras under bearbetningen toohello **HiveServer2 utdata** fönster.

    > [!NOTE]
    > hello information är hello samma som är tillgänglig från hello **Jobblogg** länka när ett jobb har slutförts.

    ![Skärmbild av logg för utdata](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Skapa ett Hive-projekt

Du kan också skapa ett projekt som innehåller flera Hive-skript. Använd ett projekt när du har relaterade skript eller toostore skript i ett system för version.

1. I Visual Studio väljer **filen**, **ny**, och sedan **projekt**.

2. Hello listan över projekt och expandera **mallar**, expandera **Azure Data Lake**, och välj sedan **HIVE (HDInsight)**. Välj hello listan över mallar **Hive exempel**. Ange ett namn och plats och välj sedan **OK**.

    ![Skärmbild av nytt projekt fönster, med Azure Data Lake, HIVE, Hive exempel och OK markerat](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

Hej **Hive exempel** projektet innehåller två skript **WebLogAnalysis.hql** och **SensorDataAnalysis.hql**. Du kan skicka dessa skript med hjälp av hello samma **skicka** knappen hello överst i fönstret hello.

## <a name="create-a-pig-project"></a>Skapa ett Pig-projekt

Medan Hive innehåller en SQL-liknande språk för att arbeta med strukturerade data, fungerar Pig genom att utföra omvandlingar på data. Pig innehåller ett språk (Pig Latin) som gör att du toodevelop en pipeline omformningar. toouse Pig med hello lokala klustret och gör följande:

1. Öppna Visual Studio och välj **filen**, **ny**, och sedan **projekt**. Hello listan över projekt och expandera **mallar**, expandera **Azure Data Lake**, och välj sedan **Pig (HDInsight)**. Välj hello listan över mallar **Pig programmet**. Ange ett namn, plats, och välj sedan **OK**.

    ![Skärmbild av nytt projekt fönster, med Azure Data Lake, Pig, Pig program och OK markerat](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. Ange följande text som hello hello hello **script.pig** filen som skapades med det här projektet.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Pig använder ett annat språk än Hive, hur du kör hello-jobben är konsekventa mellan både språk via hello **skicka** knappen. Du väljer hello listrutan bredvid **skicka** visar en dialogruta med avancerade skicka för Pig.

    ![Dialogrutan Skicka skript skärmbild](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. Hej jobbstatus och utdata visas också, hello samma som en Hive-fråga.

    ![Skärmbild av ett avslutat Pig-projekt](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>Visa jobb

Data Lake-verktyg kan du också tooeasily visa information om jobb som har körts på Hadoop. Använd hello följande steg toosee hello jobb som har körts på hello lokala klustret.

1. Från **Server Explorer**, högerklicka på hello lokala klustret och väljer sedan **visa jobb**. En lista över jobb som har skickats toohello kluster visas.

    ![Skärmbild av Server Explorer, med Visa jobb markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. Välj en tooview hello jobbinformation hello listan över jobb.

    ![Skärmbild av jobbet webbläsare med en hello jobb markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    hello information som visas är liknande toowhat som visas när du har kört en Hive- eller Pig fråga, inklusive länkar tooview hello utdata och loggar information.

3. Du kan också ändra och skicka jobbet hello härifrån.

## <a name="view-hive-databases"></a>Visa Hive-databaser

1. I **Server Explorer**, expandera hello **lokala HDInsight-kluster** post, och expandera sedan **Hive-databaser**. Hej **standard** och **xademo** databaser på hello lokala klustret visas. Expandera en databas visar hello tabeller hello-databasen.

    ![Skärmbild av Server Explorer, med databaser expanderas](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Expandera en tabell visar hello kolumner för tabellen. tooquickly visa hello data, högerklicka på en tabell och välj **visa de 100 översta raderna**.

    ![Skärmbild av Server Explorer, med tabellen expanderas och visa de 100 översta raderna markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Egenskaper för databas och tabell

Du kan visa hello egenskaperna för en databas eller tabell. Att välja **egenskaper** visar information för hello markerade objekt i egenskapsfönstret för hello. Se exempelvis hello informationen som visas i följande skärmbild hello:

![Skärmbild av egenskapsfönstret](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Skapa en tabell

toocreate en tabell, högerklicka på en databas och välj sedan **Create Table**.

![Skärmbild av Server Explorer, med Create Table markerat](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Du kan sedan skapa hello tabellen med hjälp av ett formulär. Längst ned hello i följande skärmbild hello, kan du se hello rådata HiveQL som är används toocreate hello tabell.

![Skärmbild av hello formuläret används toocreate en tabell](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Nästa steg

* [Learning hello linor av hello Hortonworks Sandbox](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-vägledning - komma igång med HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
