---
title: "aaaConnect tooAzure HDInsight med Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Lär dig hur tooinstall och använda Data Lake-verktyg för Visual Studio tooconnect tooHadoop kluster i Azure HDInsight och kör Hive-frågor."
keywords: hadoop tools,hive query,visual studio,visual studio hadoop
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: ff5819a64bebe5f4ab3cf763ce6c45c81aa34b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>Ansluta tooAzure HDInsight och köra Hive-frågor med Data Lake-verktyg för Visual Studio

Lär dig hur toouse Data Lake-verktyg för Visual Studio tooconnect tooHadoop kluster i [Azure HDInsight](hdinsight-hadoop-introduction.md) och skicka Hive-frågor. Mer information om hur du använder HDInsight finns [introduktion tooHDInsight](hdinsight-hadoop-introduction.md) och [komma igång med HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md). Mer information om hur du ansluter tooa Storm-kluster finns [utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Data Lake-verktyg för Visual Studio kan vara används tooaccess både Data Lake Analytics och HDInsight.  Hello information om Data Lake-verktyg finns [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Förutsättningar**

toocomplete den här kursen och använda hello Data Lake-verktyg i Visual Studio, behöver du hello följande:

* Ett Azure HDInsight-kluster: toocreate en, se [komma igång med Linux-baserat HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* En arbetsstation med hello följande programvara:
  
  * Windows 10, Windows 8.1, Windows 8 eller Windows 7.
  * Visual Studio 2013/2015/2017.
    
    > [!NOTE]
    > För närvarande medföljer hello Data Lake-verktyg för Visual Studio endast hello engelska versionen.
    > 
    > 

## <a name="install-data-lake-tools-for-visual-studio"></a>Installera Data Lake-verktyg för Visual Studio

Data Lake-verktyg installeras som standard för Visual Studio 2017. För äldre versioner, kan du installera den med hjälp av hello [installationsprogram för webbplattform](https://www.microsoft.com/web/downloads/). Du måste välja hello ett som matchar din version av Visual Studio. Om du inte har Visual Studio installerat kan du installera hello senaste Visual Studio Community och Azure SDK med hjälp av hello [installationsprogram för webbplattform](https://www.microsoft.com/web/downloads/):

![Data Lake-verktyg för Visual Studio Web Platform installer. ] (./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Använd installationsprogram för webbplattform tooinstall Data Lake-verktyg för Visual Studio")

## <a name="connect-tooazure-subscriptions"></a>Ansluta tooAzure prenumerationer
Data Lake-verktyg för Visual Studio kan du tooconnect tooyour HDInsight-kluster, utföra vissa grundläggande hanteringsåtgärder och köra Hive-frågor.

> [!NOTE]
> Mer information om anslutande tooa Allmänt Hadoop-kluster finns [skriva och skicka Hive-frågor med Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

**tooconnect tooyour Azure-prenumeration**

1. Öppna Visual Studio.
2. Från hello **visa** -menyn klickar du på **Server Explorer** tooopen hello Server Explorer-fönstret.
3. Expandera **Azure** och expandera därefter **HDInsight**.
   
   > [!NOTE]
   > Meddelande hello **uppgiftslista för HDInsight** ska vara öppet. Om du inte ser det klickar du på **andra fönster** från hello **visa** -menyn och klicka sedan på **fönster för uppgiftslista för HDInsight**.  
   > 
   > 
4. Ange autentiseringsuppgifterna för din Azure-prenumeration och klicka sedan på **Logga In**. Detta är bara obligatoriskt om du aldrig har anslutit toohello Azure-prenumerationen från Visual Studio på den här arbetsstationen.
5. I Server Explorer kan du se en lista över befintliga HDInsight-kluster. Om du inte har några kluster kan skapa du en med hjälp av hello Azure-portalen, Azure PowerShell eller hello HDInsight SDK. Mer information finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).
   
   ![Data Lake-verktyg för klusterlista i Visual Studio Server Explorer klusterlista](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Data Lake-verktyg för Visual Studio Server Explorer")
6. Expandera ett HDInsight-kluster. Du kommer att se **Hive-databaser**, ett standardkonto för lagring, länkade lagringskonton och **loggen för Hadoop-tjänsten**. Du kan ytterligare Expandera hello entiteter.

När du har anslutit tooyour Azure-prenumeration, kommer du att kunna toodo hello följande:

**tooconnect toohello Azure-portalen från Visual Studio**

* I Server Explorer expanderar du **Azure** > **HDInsight**, högerklickar på ett HDInsight-kluster och klickar sedan på **Hantera kluster i Azure-portalen**.

**tooask frågor och ge feedback från Visual Studio**

* Från hello **verktyg** -menyn klickar du på **HDInsight**, och klicka sedan på **MSDN-Forum** tooask frågor, eller klicka på **ge Feedback**.

## <a name="navigate-hello-linked-resources"></a>Navigera hello länkade resurser
Från Server Explorer kan se du hello standardkontot för lagring och eventuella länkade lagringskonton. Om du expanderar hello standardkontot för lagring ser hello behållare på hello storage-konto. hello standardkontot för lagring och hello standardbehållaren är markerade. Du kan också högerklicka på någon av hello behållare tooview hello innehållet.

![Data Lake-verktyg för Visual Studio serverutforskaren, länkade resurser](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "lista länkade resurser")

Du kan använda hello följande knappar tooupload, ta bort och ladda ned blobbar när du har öppnat en behållare:

![Bloboperationer i Server Explorer för Data Lake-verktyg för Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "ladda upp, ta bort och hämta blob")

## <a name="run-a-hive-query"></a>Köra en Hive-fråga
[Apache Hive](http://hive.apache.org) är en datalagerinfrastruktur som bygger på Hadoop och som används för att tillhandahålla datasammanfattning, frågor och analys. Data Lake-verktyg för Visual Studio stöder körning av Hive-frågor från Visual Studio. Mer information om Hive finns i [Använda Hive med HDInsight](hdinsight-use-hive.md).

Det är tidskrävande tootest Hive-skript mot ett HDInsight-kluster. Det kan ta flera minuter eller ännu längre tid. Data Lake-verktyg för Visual Studio kan validera Hive-skript lokalt utan anslutande tooa aktivt kluster.

Data Lake-verktyg för Visual Studio låter också användare toosee vad som finns i hello Hive-jobb genom att samla in och visa hello YARN-loggar för specifika Hive-jobb.

### <a name="view-hello-hivesampletable"></a>Visa hello **hivesampletable**
Alla HDInsight-kluster har en Hive-exempeltabell med namnet *hivesampletable*. Vi använder den här tabellen tooshow du hur toolist Hive-tabeller, visa hello tabellscheman och listar hello rader i hello Hive tabell.

**toolist Hive-tabeller och visa Hive-tabellschema**

1. Från **Server Explorer**, expandera **Azure** > **HDInsight** > Hej valfritt kluster > **Hive-databaser**  >  **Standard** > **hivesampletable** toosee hello tabellens schema.
2. Högerklicka på **hivesampletable**, och klicka sedan på **visa de 100 översta raderna** toolist hello rader. Det är likvärdiga toorunning hello följande Hive-fråga med hjälp av ODBC-drivrutinen:
   
     VÄLJ * FRÅN hivesampletable GRÄNS 100
   
   Du kan anpassa hello radantal.
   
   ![Data Lake-verktyg: HDInsight Hive Visual Studio-schemafråga](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Hive-fråga, resultat")

### <a name="create-hive-tables"></a>Skapa Hive-tabeller
Du kan använda hello GUI toocreate en Hive-tabell eller använda Hive-frågor. Information om hur du använder Hive-frågor finns i [Köra Hive-frågor](#run.queries).

**toocreate en Hive-tabell**

1. Gå till **Server Explorer** och expandera **Azure** > **HDInsight-kluster** ett HDInsight-kluster > **Hive-databaser** och högerklicka sedan på **Standard** och klicka på **Skapa tabell**.
2. Konfigurera hello tabell.
3. Klicka på **Create Table** toosubmit hello jobbet toocreate hello nya Hive-tabellen.
   
    ![Data Lake-verktyg: HDinsight visual studio tools, skapa Hive-tabell](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Skapa Hive-tabell")

### <a name="run.queries"></a>Validera och köra Hive-frågor
Det finns två sätt toocreate och kör Hive-frågor:

* Skapa ad hoc-frågor
* Skapa ett Hive-program

**toocreate, validera och köra ad hoc-frågor**

1. Gå till **Server Explorer**, expandera **Azure** och expandera sedan **HDInsight-kluster**.
2. Högerklicka på hello klustret där du vill toorun hello fråga och klicka sedan på **skriva en Hive-fråga**.
3. Ange hello Hive-frågor. Meddelande hello Hive-redigeraren stöder IntelliSense. Data Lake-verktyg för Visual Studio stöder inläsning hello fjärrmetadata när du redigerar ditt Hive-skript. Till exempel när du skriver ”SELECT * FROM”, hello IntelliSense en lista över hello alla föreslagna tabellnamn. Om du har angett ett tabellnamn anges hello kolumnnamn med hello IntelliSense. hello-verktygen stöder nästan alla Hive DML-instruktioner, underfrågor, och hello inbyggda UDF.
   
    ![Data Lake-verktyg: HDInsight Visual Studio Tools IntelliSense](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Data Lake-verktyg: HDInsight Visual Studio Tools IntelliSense](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Endast hello metadata hello kluster som valts i verktygsfältet för HDInsight föreslås.
   > 
   > 
4. (Valfritt): Klicka på **Validera skriptet** toocheck hello syntaxfel i skriptet.
   
    ![Data Lake-verktyg: Data Lake-verktyg för lokal Visual Studio-validering](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Validera skript")
5. Klicka på **Skicka** eller **Skicka (avancerat)**. Med avancerade alternativet för att skicka hello, konfigurerar du **jobbnamn**, **argument**, **ytterligare konfigurationer**, och **Statuskatalog**för hello skript:
   
    ![HDInsight Hadoop hive-fråga](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Sänd frågor")
   
    När du skickar hello jobb visas en **Hive-jobbsammanfattning** fönster.
   
    ![Sammanfattning av en HDInsight Hadoop Hive-fråga](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Hive-jobbsammanfattning")
6. Använd hello **uppdatera** knappen tooupdate hello status tills hello ändringar av jobbstatus för**slutförd**.
7. Klicka på hello hello nedre toosee hello följande länkar: **Jobbfråga**, **Jobbutdata**, **jobblogg**, eller **Yarn-logg**.

**toocreate och köra en Hive-lösning**

1. Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.
2. Välj **HDInsight** hello till vänster och välj **Hive-program** anger hello egenskaper i hello mellersta rutan och klicka sedan på **OK**.
   
    ![Data Lake-verktyg: HDInsight Visual Studio Tools nya hive-projekt](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Skapa Hive-program från Visual Studio")
3. Från **Solution Explorer**, dubbelklicka på **Script.hql** tooopen den.
4. toovalidate hello Hive-skript, kan du klicka på hello **Validera skriptet** knappen eller högerklicka på hello skriptet i hello Hive-redigeraren och klicka sedan på **Validera skriptet** hello snabbmenyn.

### <a name="view-hive-jobs"></a>Visa Hive-jobb
Du kan visa jobbfrågor, jobbutdata, jobbloggar och Yarn-loggar för Hive-jobb. Mer information finns i hello föregående skärmbild.

hello den senaste versionen av hello verktyg kan du toosee vad som finns i Hive-jobb genom att samla in och visa YARN-loggar. En YARN-logg kan hjälpa dig att undersöka prestandaproblem. Mer information om hur HDInsight samlar in YARN-loggar finns i [Access HDInsight Application Logs Programmatically](hdinsight-hadoop-access-yarn-app-logs.md) (Komma åt HDInsight-programloggar via programmering).

**tooview Hive-jobb**

1. Gå till **Server Explorer**, expandera **Azure** och expandera sedan **HDInsight**.
2. Högerklicka på ett HDInsight-kluster och klicka sedan på **Visa jobb**. En lista över hello Hive-jobb som körts på hello kluster visas.
3. Klicka på ett jobb i hello jobbet listan tooselect den och sedan använda hello **Hive-jobbsammanfattning** fönstret tooopen **Jobbfråga**, **Jobbutdata**, **Jobblogg**, eller **Yarn-logg**.
   
    ![Data Lake-verktyg: HDInsight Visual Studio Tools, visa Hive-jobb](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Visa Hive-jobb")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Snabbare Hive-sökvägsexekvering via HiveServer2
> [!NOTE]
> Den här funktionen fungerar bara på HDInsight-kluster av version 3.2 eller senare.
> 
> 

hello Data Lake-verktyg används toosubmit Hive-jobb via [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (även kallat Templeton). Det tog en lång tid tooreturn Jobbdetaljer och felinformation.
I ordning toosolve prestandaproblemet hello Data Lake-verktygen körs Hive-jobb direkt i hello klustret via HiveServer2 och kringgår RDP/SSH.
Tillägg toobetter prestanda kan användare också visa Hive i Tez-diagram och hello Aktivitetsinformation.

För HDInsight-kluster av version 3.2 eller senare visas knappen **Kör via HiveServer2**:

![Data Lake visual studio Tools, kör via hiveserver2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Köra Hive-frågor med HiveServer2")

Och du kan se hello loggar strömmas tillbaka i realtid och se hello jobbdiagram om hello Hive-fråga körs i Tez.

![Data Lake visual studio tools för snabb sökväg för hive-körning](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Visa jobbgrafer")

**Skillnaden mellan att köra frågor via HiveServer2 och skicka frågor via WebHCat**

Även om det finns många fördelar med att köra frågor via HiveServer2 finns det också flera begränsningar. Vissa hello begränsningar är inte lämplig för användning i produktion. hello visar följande tabell hello skillnader:

|  | Köra via HiveServer2 | Skicka via WebHCat |
| --- | --- | --- |
| Köra frågor |Eliminerar hello pålägget i WebHCat (som startar ett MapReduce-jobb med namnet ”TempletonControllerJob”). |Så länge en fråga körs via WebHCat startar WebHCat MapReduce-jobbet, vilket medför en ytterligare fördröjning. |
| Dataströmmen loggas tillbaka |I nära realtid. |Hej körningsloggar för jobbet är bara tillgängliga när hello-jobbet är klart. |
| Visa jobbhistorik |Om en fråga körs via HiveServer2 bevaras inte dess jobbhistorik (jobblogg, jobbutdata). hello program kan visas i YARN-Användargränssnittet med begränsad information. |Om en fråga körs via WebHCat bevaras dess jobbhistorik (jobblogg, jobbutdata) och kan visas med hjälp av Visual Studio/HDInsight SDK/PowerShell. |
| Stäng fönster |Köra via HiveServer2 är en ”synkron” metod, så du måste hålla hello windows öppna; Om hello fönstren stängs avbryts hello Frågekörningen. |Skicka via WebHCat är en ”asynkron” metod, så du kan skicka hello frågan via WebHCat och stänga Visual Studio. Du kan gå tillbaka och se hello resultaten när som helst. |

### <a name="tez-hive-job-performance-graph"></a>Tez-diagram över Hive-jobbprestanda
hello Data Lake-verktygen stöder visning av prestandadiagram för hello Hive-jobb som körts av Tez-Körningsmotor för hello. Information om hur du aktiverar Tez finns i [Använda Hive i HDInsight](hdinsight-use-hive.md). När du har skickat hello registreringsdata visar jobb i Visual Studio, Visual Studio diagrammet när hello jobbet har slutförts.  Du kan behöva tooclick hello **uppdatera** knappen tooget hello senaste jobbstatus.

> [!NOTE]
> Den här funktionen är endast tillgänglig för HDInsight-kluster av senare version än 3.2.4.593 och fungerar bara för slutförda jobb (om du skickat jobbet via WebHCat visas det här diagrammet när du kör frågan via HiveServer2). Detta fungerar både i Windows- och Linux-baserade kluster.
> 
> 

![hadoop hive tez-prestandadiagram](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Jobbstatus")

toohelp du förstå din Hive fråga bättre, hello Verktyg Lägg till hello Hive Operator-vyn i den här versionen. Behöver du bara toodouble genom att klicka på hello formhörnen hello jobbdiagram och du kan se alla hello operatorer innanför hello hörn. Du kan även hovra över en viss operator tooview mer information om den här operatorn.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Vy för uppgiftskörning för Hive-jobb på Tez
Hej vy för uppgiftskörning för Hive jobb på Tez kan användas tooget strukturerad och visualiserad information för Hive-jobb och få mer jobbinformation. När det uppstår prestandaproblem kan använda du hello visa tooget ytterligare information. Hur varje uppgift fungerar och hello detaljerad exempelvis information om varje uppgift (data lästa/skrivna, schema/Starttid/Sluttid o.s.v.), så att du kan finjustera jobbkonfigurationer eller systemarkitektur baserat på hello visualiseras information.

![Data Lake visual studio tools, vy för uppgiftskörning](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Vy för uppgiftskörning")

## <a name="run-pig-scripts"></a>Köra Pig-skript
Data Lake-verktyg för Visual Studio kan du skapa och skicka Pig-skript tooHDInsight kluster. Användare kan skapa ett Pig-projekt från mall och skicka hello skriptet tooHDInsight kluster.

## <a name="feedbacks--known-issues"></a>Feedback och kända problem
* För närvarande visas resultaten från HiveServer2 med endast text, vilket inte är idealiskt. Vi arbetar på att åtgärda detta.
* Om hello resultat har startats med NULL-värden visas för närvarande hello resultat inte. Vi har löst detta och om du blockeras på det här problemet är ledigt toodrop oss en e-post eller kontakta supportteam.
* Hej HQL-skript som skapas av Visual Studio kodas beroende på användarens lokala regioninställning. Kanske inte kan köras korrekt om användaren överför hello skriptet toocluster som binary.

## <a name="next-steps"></a>Nästa steg
I den här artikeln får du lära dig hur tooconnect tooHDInsight kluster från Visual Studio med hello Data Lake (HDInsight)-verktygspaketet och hur toorun en Hive-fråga. Mer information finns i:

* [Använda Hadoop Hive i HDInsight](hdinsight-use-hive.md)
* [Komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Skicka Hadoop-jobb i HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Analysera Twitter-data med Hadoop i HDInsight](hdinsight-analyze-twitter-data.md)

