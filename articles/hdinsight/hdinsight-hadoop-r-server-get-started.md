---
title: "aaaGet igång med R Server på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toocreate en Apache Väck på HDInsight-kluster som innehåller R-Server och skicka ett R-skript på hello klustret."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: f7e418bbac48eee080a4b4cfbb33e246324ea5c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a>Kom igång med R Server på HDInsight

HDInsight innehåller en R Server alternativet toobe integreras i ditt HDInsight-kluster. Det här alternativet kan du R toouse Spark och MapReduce toorun distribuerade beräkningar. I det här dokumentet beskrivs hur toocreate en R Server på HDInsight-kluster och sedan kör ett R-skriptet som visar med Spark för distribuerade R-beräkningar.


## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**: Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration. Gå toohello artikel [hämta Microsoft kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) för mer information.
* **En klient SSH (Secure Shell)**: en SSH-klienten används tooremotely ansluta toohello HDInsight-kluster och köra kommandon direkt på hello klustret. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).
* **SSH-nycklar (valfritt)**: du kan skydda hello SSH konto som används för tooconnect toohello kluster med hjälp av ett lösenord eller en offentlig nyckel. Använder ett lösenord som är enklare och gör att du tooget igång utan att behöva toocreate offentliga/privata nyckelpar med en nyckel. Det är dock säkrare att använda en nyckel.

> [!NOTE]
> hello stegen i det här dokumentet förutsätter att du använder ett lösenord.


## <a name="automated-cluster-creation"></a>Skapa kluster automatiskt

Du kan automatisera hello skapa HDInsight R Servers med Azure Resource Manager-mallar, hello SDK och även PowerShell.

* toocreate ett R-Server med en Azure Resource Manager-mall finns [distribuera ett R server HDInsight-kluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* toocreate ett R-servern med hjälp av hello .NET SDK finns [skapa Linux-baserade kluster i HDInsight med hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* toodeploy R Server med hjälp av powershell, se hello artikel på [skapar en R Server på HDInsight med PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-hello-cluster-using-hello-azure-portal"></a>Skapa hello kluster med hello Azure-portalen

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Välj **NYTT** -> **Information + analys**, -> **HDInsight**.

    ![Bild som visar hur du skapar ett nytt kluster](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. I hello **Snabbregistrering** upplevelse, ange ett namn för hello kluster i hello **klusternamnet** fältet. Om du har flera Azure-prenumerationer, Använd hello **prenumeration** post tooselect hello en du vill toouse.

    ![Val av klustrets namn och prenumeration](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. Välj **kluster typen** tooopen hello **klusterkonfigurationen** bladet. På hello **klusterkonfigurationen** bladet välj hello följande alternativ:

    * **Klustertyp**: R Server
    * **Version**: Välj hello version av R Server tooinstall på hello klustret. Hej för närvarande tillgängliga versionen är ***R Server 9.1 (HDI 3,6)***. Viktig information för hello tillgängliga versioner för R Server är tillgängliga [här](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **R Studio community edition för R Server**: den här webbläsarbaserad IDE installeras som standard på hello kantnod. Om du föredrar toonot har installerats och sedan avmarkera kryssrutan för hello. Om du väljer toohave installeras det hello URL: en för att komma åt hello RStudio Server-inloggning finns på ett portalprogram blad för klustret när den skapas.
    * Lämna hello andra alternativ på hello standardvärden och använda hello **Välj** knappen toosave hello typ av kluster.

        ![Skärmbild av klustertypblad](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. Ange ett **inloggningsnamn** och **inloggningslösenord** för klustret.

    Ange ett **SSH-användarnamn**. SSH är används tooremotely ansluta toohello klustret med en **SSH (Secure Shell)** klienten. Du kan antingen ange hello SSH-användare i den här dialogrutan eller hello klustret har skapats (i hello konfigurationsfliken för hello kluster). R Server är konfigurerad tooexpect en **SSH-användarnamn** av ”remoteuser”.  **Om du använder ett annat användarnamn måste du utföra ytterligare ett steg efter hello klustret skapas.**

    Lämna hello ikryssad för **använda samma lösenord som klusterinloggning** toouse **lösenord** som hello autentisering skriva om du föredrar att användning av en offentlig nyckel.  Du behöver ett offentligt/privat nyckelpar tooaccess R Server på hello klustret via en fjärransluten klient som till exempel RTVS, RStudio eller ett annat skrivbord IDE. Om du installerar hello RStudio Server community edition måste toochoose ett SSH-lösenord.     

    toocreate och använder en offentlig/privat nyckelpar, avmarkera **använda samma lösenord som klusterinloggning** och välj sedan **offentliga nyckel** och fortsätt sedan som följer. Anvisningarna förutsätter att du har Cygwin med ssh-keygen eller motsvarande installerat.

    * Generera ett offentligt/privat nyckelpar från hello kommandotolk på din bärbara dator:

        ssh-keygen -t rsa -b 2048

    * Följ hello fråga tooname en nyckelfil och ange en lösenfras för extra säkerhet. Skärmen ska se ut ungefär hello följande bild:

        ![SSH-kommandorad i Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * Det här kommandot skapar en fil för privat nyckel och en offentlig nyckelfil under hello namn < privat-nyckel-filnamn > pub, till exempel furiosa och furiosa.pub.

        ![SSH-katalog](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * Ange hello fil för offentlig nyckel (&#42;. pub) när tilldela HDI kluster autentiseringsuppgifter och slutligen bekräftar din resursgrupp och region och välj **nästa**.

        ![Bladet Autentiseringsuppgifter](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * Ändra behörigheter för hello privata keyfile på din bärbara dator:

        chmod 600 <private-key-filename>

   * Använd hello-fil för privat nyckel med SSH för fjärrinloggning:

        ssh –i <private-key-filename> remoteuser@<hostname public ip>

      Eller som en del hello definition i Hadoop-Spark compute-kontexten för R Server på hello-klienten. Se hello **med hjälp av Microsoft R-Server som en klient i Hadoop** underavsnitt i [skapa en Compute kontext för Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).

6. hello Snabbregistrering övergångar toohello **lagring** bladet tooselect hello lagringskonto inställningar toobe används för hello primära platsen för hello HDFS filsystem används av hello klustret. Välj antingen ett nytt eller ett befintligt Azure Storage-konto eller Data Lake-lagringskonto.

    - Om du väljer ett Azure Storage-konto, ett befintligt lagringskonto har valts genom att välja **Välj ett lagringskonto** och sedan välja relevanta hello-kontot. Skapa ett nytt konto med hjälp av hello **Skapa nytt** länken i hello **Välj ett lagringskonto** avsnitt.

      > [!NOTE]
      > Om du väljer **ny** du måste ange ett namn för hello nytt lagringskonto. En grön bock visas om hello namn accepteras.

      Hej **standard behållaren** standardvärden hello klustrets toohello namn. Lämna standardinställningen som hello-värde.

      Om ett nytt konto lagringsalternativ valdes fråga tooselect **plats** är angivna tooselect vilken region toocreate hello storage-konto.  

         ![Bladet Datakälla](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Att välja hello plats för hello standarddatakälla anger också hello platsen för hello HDInsight-kluster. hello klustret och standard datakällan måste vara i hello samma region.

    - Om du vill toouse en befintlig Data Lake Store, Välj hello ADLS storage-konto toouse och lägga till klustret hello *Lägg till* identitet tooyour klustret tooallow åtkomst till toohello store. Mer information om den här processen finns i [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (Skapa HDInsight-kluster med Data Lake Store med Azure Portal).

    Använd hello **Välj** knappen toosave hello datakällkonfiguration.


7. Hej **sammanfattning** bladet visar toovalidate dina inställningar. Här kan du ändra din **klusterstorleken** toomodify hello antal servrar i klustret och även ange någon **skript åtgärder** du vill toorun. Om du inte vet att du behöver större, lämna hello antalet arbetarnoder på hello standardvärdet `4`. hello uppskattade kostnaden för hello kluster visas inom hello-bladet.

    ![klustersammanfattning](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Om det behövs, du kan ändra storlek på ditt kluster senare via hello Portal (**klustret** -> **inställningar** -> **kluster**) tooincrease eller minska hello antalet arbetarnoder.  Storleksändring av den här kan vara användbart för tomgång ned hello klustret som, eller för att lägga till kapacitetsbehov toomeet hello större uppgifter.
   >
   >

   Vissa faktorer tookeep i åtanke när du ändrar storlek på ditt kluster och hello datanoder hello kantnod inkluderar:  

   * hello prestanda för den distribuerade R Server analyser på Spark är proportionell toohello antalet arbetarnoder när hello informationen är stor.  

   * hello prestanda för R Server analys är linjär i hello storleken på data som analyseras. Exempel:  

     * För små toomodest data är prestanda bäst när analyseras i en kontext som lokala beräkning på hello kantnod.  Mer information om hello scenarier som hello lokala och Spark compute kontexter fungerar bäst, se beräkning kontexten alternativ för R Server på HDInsight.<br>
     * Om du loggar in toohello kantnod och kör din R-skriptet kommer alla utom hello ScaleR rx-funktioner utförs <strong>lokalt</strong> på hello kantnod. Så hello minne och antalet kärnor för hello kantnod ska ändras i enlighet därmed. hello detsamma gäller om du använder R Server på HDI som en fjärransluten beräknings-kontext från din bärbara dator.

     ![Bladet Nodprisnivåer](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     Använd hello **Välj** knappen toosave hello nod priser konfiguration.

   Det finns även en länk till att **ladda ned mall och parametrar**. Klicka på den här länken toodisplay skript som kan använda tooautomate hello skapandet av ett kluster med hello markerade konfigurationsobjekt. Dessa skript är också tillgängliga på hello Azure portal post för klustret när den har skapats.

   > [!NOTE]
   > Det tar en stund innan hello klustret toobe skapas vanligtvis cirka 20 minuter. Använda hello panelen på hello startsidan eller hello **meddelanden** transaktionen på hello vänsterkant hello sidan toocheck på hello-processen.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-toorstudio-server"></a>Ansluta tooRStudio Server

Om du har valt tooinclude RStudio Server community edition i installationen, kan du komma åt hello RStudio inloggning via två olika metoder.

1. Gå toohello följande URL (där **KLUSTERNAMN** är hello namnet på hello-kluster som du har skapat):

    https://**KLUSTERNAMN**.azurehdinsight.net/rstudio/

2. Öppna hello post för klustret i hello Azure-portalen väljer hello **R Server instrumentpaneler** snabb länk och sedan välja hello **R Studio instrumentpanelen**:

     ![Instrumentpanelen för hello R studio](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Instrumentpanelen för hello R studio](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Oavsett hello-metod som används för måste hello första gången du loggar in tooauthenticate två gånger.  Ange hello på hello första autentiseringen, *kluster Admin userid* och *lösenord*. Ange hello i Kommandotolken hello andra *SSH userid* och *lösenord*. Efterföljande loggen moduler kräver endast hello *SSH-lösenordet* och *userid*.

<a name="connect-to-edge-node"></a>
## <a name="connect-toohello-r-server-edge-node"></a>Ansluta toohello R-serverns kantnod

Anslut tooR serverns kantnod hello HDInsight-klustret via SSH med hello-kommando:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Du kan hitta hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` adressen i hello Azure-portalen genom att välja klustret sedan **alla inställningar** -> **appar** -> **RServer**. Detta visar hello SSH slutpunktsinformation för hello kantnod.
>
> ![Bild av hello SSH-slutpunkten för hello kantnod](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

Om du har använt ett lösenord toosecure SSH-användarkontot, är du tillfrågas tooenter den. Om du använder en offentlig nyckel måste du kanske toouse hello `-i` parametern toospecify hello motsvarande privata nyckel. Exempel:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Mer information finns i [ansluta tooHDInsight (Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

När du är ansluten, kommer du till en fråga liknande toohello följande:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Aktivera flera samtidiga användare

Du kan aktivera flera samtidiga användare genom att lägga till fler användare för hello kantnod vilka hello RStudio community version körs.

När du skapar ett HDInsight-kluster måste du ange två användare, en HTTP-användare och en SSH-användare:

![Samtidig användare 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- **Klustret inloggning användarnamn**: en HTTP-användare för autentisering via hello HDInsight gateway som används tooprotect hello HDInsight-kluster du skapade. Den här HTTP-användare är används tooaccess hello Ambari UI, YARN-Användargränssnittet, samt andra UI-komponenter.
- **Secure Shell (SSH) användarnamn**: ett SSH användaren tooaccess hello klustret via secure shell. Den här användaren är en användare i hello Linux-system för alla hello huvudnoderna arbetarnoder och kant-noder. Så att du kan använda secure shell tooaccess någon hello noder i ett kluster.

hello R Studio Server Community-version som används i hello Microsoft R Server på HDInsight-kluster typen accepterar endast Linux-användarnamn och lösenord som en mekanism för inloggning. Du kan inte skicka token. Om du har skapat ett nytt kluster och vill toouse R Studio tooaccess, behöver du toolog i två gånger.

- Logga först in hello HTTP användarens autentiseringsuppgifter via hello HDInsight Gateway: 

    ![Samtidig användare 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- Använd sedan hello SSH användarens autentiseringsuppgifter toolog i tooRStudio:
  
    ![Samtidig användare 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

För närvarande kan du bara skapa ett SSH-användarkonto när du etablerar ett HDInsight-kluster. Så tooenable flera användare tooaccess Microsoft R Server på HDInsight-kluster måste toocreate ytterligare användare i hello Linux-system.

Eftersom RStudio Server Community körs på hello klustrets kantnod finns här flera steg:

1. Använd hello skapat SSH användaren toolog i toohello kantnod
2. Lägg till fler Linux-användare på kantnoden
3. Använda RStudio gruppversion med hello som skapats av användare

### <a name="step-1-use-hello-created-ssh-user-toolog-in-toohello-edge-node"></a>Steg 1: Använd hello skapat SSH användaren toolog i toohello kantnod

Hämtar ett SSH-verktyg (till exempel Putty) och använder hello befintliga SSH användaren toolog i. Följ sedan instruktionerna i hello [ansluta tooHDInsight (Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello kantnod. hello edge nodadressen för R Server på HDInsight-kluster är: *klusternamn-ed-ssh.azurehdinsight.net*


### <a name="step-2-add-more-linux-users-in-edge-node"></a>Steg 2: Lägg till fler Linux-användare på kantnoden

tooadd användaren toohello kantnod köra hello-kommandon:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Du bör se hello efter objekt som returneras: 

![Samtidig användare 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

När du uppmanas att ”aktuella Kerberos lösenord”:, trycker du bara på **RETUR** tooignore den. Hej `-m` alternativet i `useradd` kommando visar att hello systemet skapar en arbetsmapp för hello användare, vilket krävs för RStudio gruppversion.


### <a name="step-3-use-rstudio-community-version-with-hello-user-created"></a>Steg 3: Använd RStudio gruppversion med hello som skapats av användare

Använd hello användaren som skapade toolog i tooRStudio:

![Samtidig användare 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

Observera att RStudio anger att du använder hello ny användare (här, till exempel *sshuser6*) toolog i hello kluster: 

![Samtidig användare 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

Du kan också logga in med hello ursprungliga autentiseringsuppgifter (som standard är det *sshuser*) samtidigt från ett nytt webbläsarfönster.

Du kan skicka ett jobb med hjälp av ScaleR-funktioner. Här är ett exempel på hello kommandon som används för toorun ett jobb:

    # Set hello HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data toohello tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot tooload hello data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create hello directory.
    rxHadoopMakeDir(inputDir)

    # Copy hello data from source tooinput.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define hello HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for hello airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all hello column names.
    varNames <- names(airlineColInfo)

    # Define hello text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define hello text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify hello formula toouse.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define hello Spark compute context.
    mySparkCluster <- RxSpark()

    # Set hello compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


Lägg märke till att hello jobb som skickats under olika användarnamn i YARN-Användargränssnittet:

![Samtidig användare 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

Observera också att hello nyligen tillagda användare har inte behörighet för rot i Linux-system, men de ha hello samma åt tooall hello filer i hello HDFS och WASB Fjärrlagring.


<a name="use-r-console"></a>
## <a name="use-hello-r-console"></a>Använda hello R-konsolen

1. Använd följande kommandokonsolen toostart hello R hello från hello SSH-sessionen:  

        R

2. Du bör se utdata liknande toohello följande:
    
    R version 3.2.2 (2015-08-14)--”eld” Copyright (C) 2015 hello R Foundation för statistisk databehandling plattform: x86_64-pc-linux-gnu (64-bitars)

    R is free software and comes with ABSOLUTELY NO WARRANTY.
    Är du Välkommen tooredistribute det vissa villkor.
    Type 'license()' or 'licence()' for distribution details.

    Natural language support but running in an English locale

    R is a collaborative project with many contributors.
    Ange 'contributors()' för mer information och 'citation()' på hur toocite R eller R-paket i publikationer.

    Ange 'demo()' för vissa demonstrationer, help() om du för onlinehjälp eller help.start() om du för en HTML-webbläsaren gränssnittet toohelp.
    Skriv ”q()' tooquit R.

    Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation

    Type 'readme()' for release notes.
    >

3. Från hello `>` kommandotolk, kan du ange R-kod. R server innehåller paket som gör att du tooeasily interagera med Hadoop och köra distribuerade beräkningar. Till exempel använda hello efter kommandot tooview hello roten för hello standardfilsystem för hello HDInsight-kluster:

    rxHadoopListFiles("/")

4. Du kan också använda hello WASB style-adressering.

    rxHadoopListFiles("wasb:///")


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Använda R Server på HDI från en fjärrinstans av Microsoft R Server eller Microsoft R Client

Det är möjligt tooset in åtkomst toohello HDI Hadoop Spark beräkning kontext från en fjärrinstans av Microsoft R Server eller Microsoft R-klienten körs på en stationär eller bärbar dator. Se **med hjälp av Microsoft R-Server som en klient i Hadoop** underavsnitt i hello [skapar en Compute kontext för Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md). toodo, du behöver toospecify hello följande alternativ när du definierar hello RxSpark beräkning kontext på din bärbara dator: hdfsShareDir, shareDir, sshUsername, sshHostname sshSwitches, och sshProfileScript. Exempel:


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a>Använda en beräkningskontext

En beräknings-kontext kan toocontrol om beräkning utföras lokalt på hello kantnod eller distribueras över hello noderna i hello HDInsight-kluster.

1. Använd hello följande kod tooload exempeldata i hello standardlagring för HDInsight från RStudio Server eller hello R-konsolen (i en SSH-session):

        # Set hello HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data toohello tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot tooload hello data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make hello directory
        rxHadoopMakeDir(inputDir)

        # Copy hello data from source tooinput
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Nu ska vi skapa vissa data info och definiera två datakällor så att vi kan arbeta med hello data.

        # Define hello HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for hello airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all hello column names
        varNames <- names(airlineColInfo)

        # Define hello text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define hello text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula toouse
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Vi kör en logistic regression över hello data med hello lokala beräkning kontext.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Du bör se utdata som slutar med rader liknande toohello följande:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Nu ska vi kör hello samma logistic regression med hello Spark kontext. hello Spark kontexten distribuerar hello bearbetning, över alla hello arbetarnoder i hello HDInsight-kluster.

        # Define hello Spark compute context
        mySparkCluster <- RxSpark()

        # Set hello compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Du kan också använda MapReduce toodistribute beräkning över klusternoder. Mer information om beräkningskontexter finns i [Alternativ för beräkningskontexter för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).


## <a name="distribute-r-code-toomultiple-nodes"></a>Distribuera R kod toomultiple noder

Med R Server kan du enkelt ta befintliga R-koden och kör den över flera noder i klustret hello med hjälp av `rxExec`. Det här är praktiskt när du gör en parameterrensning eller simuleringar. hello följande kod är ett exempel på hur toouse `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Om du fortfarande använder hello Spark eller MapReduce kontext, det här kommandot returnerar hello nodnamn värdet för hello arbetarnoder hello koden `(Sys.info()["nodename"])` körs på. Till exempel på ett kluster med fyra noder du förväntar dig tooreceive utdata liknande toohello följande:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="accessing-data-in-hive-and-parquet"></a>Få åtkomst till data i Hive och Parquet

En funktion som är tillgängliga i R Server 9.1 kan direktåtkomst toodata i Hive och parkettgolv för användning av ScaleR funktioner i hello Spark beräknings-kontexten. Dessa funktioner är tillgängliga via nya ScaleR datakälla funktioner anropade RxHiveData och RxParquetData som fungerar med hjälp av Spark SQL tooload data direkt till en Spark DataFrame för analys av ScaleR.  

hello följande kod innehåller vissa exempelkod för användning av hello nya funktioner:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close hello Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Ytterligare information om användning av dessa nya funktioner finns i hello direkthjälpen i R Server med hjälp av hello `?RxHivedata` och `?RxParquetData` kommandon.  


## <a name="install-additional-r-packages-on-hello-edge-node"></a>Installera ytterligare R-paket på hello kantnod

Om du vill att tooinstall ytterligare R-paket på hello kantnod, kan du använda `install.packages()` direkt inifrån hello R-konsolen när anslutna toohello kant nod via SSH. Om du behöver tooinstall R-paket på hello worker klusternoder hello, måste du använda en skriptåtgärd.

Skriptåtgärder är Bash-skript som används toomake configuration ändringar toohello HDInsight-kluster eller tooinstall ytterligare programvara, till exempel ytterligare R-paket. tooinstall ytterligare paket med hjälp av en skriptåtgärd Använd hello följande steg:

> [!IMPORTANT]
> Med hjälp av skriptåtgärder tooinstall ytterligare R-paket kan bara användas när hello klustret har skapats. Använd inte den här proceduren när klustret skapas som hello skript förlitar sig på R Server som installerats och konfigurerats helt.
>
>

1. Från hello [Azure-portalen](https://portal.azure.com), Välj din R-Server på HDInsight-kluster.

2. Från hello **inställningar** bladet väljer **skriptåtgärder** och sedan **skicka nya** toosubmit nya skriptåtgärden.

   ![Bild på bladet med skriptåtgärder](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. Från hello **skicka skriptåtgärden** bladet ange hello följande information:

   * **Namnet**: ett eget namn tooidentify skriptet

   * **Bash-skript-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Huvud**: det här alternativet ska vara **avmarkerat**

   * **Arbetare**: det här alternativet ska vara **markerat**

   * **Edge-noder**: det här alternativet ska vara **avmarkerat**

   * **Zookeeper**: det här alternativet ska vara **avmarkerat**

   * **Parametrarna**: hello R-paket toobe installerad. Till exempel, `bitops stringr arules`

   * **Spara den här skriptåtgärden ...**: det här alternativet ska vara **markerat**  

   > [!NOTE]
   > 1. Som standard installeras alla R-paket från en ögonblicksbild av hello Microsoft MRAN databasen är konsekvent med hello version av R-Server som har installerats. Om du vill tooinstall nyare versioner av paket, finns en risk för inkompatibilitet. Men den här typen av installation är möjligt genom att ange `useCRAN` som hello första elementet i hello paketet lista, till exempel `useCRAN bitops, stringr, arules`.  
   > 2. För vissa R-paket krävs ytterligare Linux-bibliotek. För enkelhetens skull har vi förinstallerat hello beroenden som krävs av hello översta 100 populäraste R-paket. Om hello R-paket som du installerar kräver bibliotek utöver dessa måste sedan du hämta hello grundläggande skript används här och lägga till steg tooinstall hello bibliotek. Du måste sedan ladda upp hello ändrade skript tooa offentliga blob-behållaren i Azure storage och använda hello ändrade skriptet tooinstall hello paket.
   >    Mer information om hur du utvecklar skriptåtgärder finns i [Skriptåtgärdsutveckling](hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Lägga till en skriptåtgärd](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Välj **skapa** toorun hello skript. Hello R-paket är tillgängliga på alla arbetarnoder när hello skriptet har slutförts.


## <a name="using-microsoft-r-server-operationalization"></a>Använda Microsoft R Server-driftsättningen

När dina datamodellering är klar, kan du operationalisera hello modellen toomake förutsägelser. tooconfigure för Microsoft R Server operationalization utför hello följande steg:

Första, ssh till hello kantnod. Exempel: 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

När du använder ssh, ändra katalogen hello relevant version och sudo hello dotnet-dll för: 

- Microsoft R-server 9.1:

    cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

- Microsoft R-server 9.0:

    cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

tooconfigure Microsoft R Server operationalization med en konfiguration med en ruta hello följande:

1. Välj ”Configure R Server for Operationalization” (Konfigurera R-server för driftsättning)
2. Välj ”A. One-box (web + compute nodes)” (En konfiguration (webb- och beräkningsnoder))
3. Ange ett lösenord för hello **admin** användare

![en enda driftsättning](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Du kan välja att utföra diagnostiska kontroller genom att köra följande diagnostiska test:

1. Välj ”6. Run diagnostic tests” (Kör diagnostiktest)
2. Välj ”A. Testkonfiguration”
3. Ange användarnamnet ”admin” och lösenordet från föregående konfigurationssteg
4. Bekräfta övergripande hälsa = skicka
5. Avsluta hello administrationsverktyget
6. Avsluta SSH

![Diagnostik för driftsättning](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>**Långa fördröjningar när webbtjänster utnyttjas på Spark**
>
>Om det uppstår långa fördröjningar när försök tooconsume en webbtjänst med mrsdeploy funktioner i en kontext för beräkning av Spark kan behöva du tooadd vissa mappar som saknas. hello Spark-program tillhör tooa användare som kallas ”*rserve2*' när den anropas från en webbtjänst med hjälp av mrsdeploy funktioner. toowork runt problemet:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


I det här skedet har hello konfigurationen för Operationalization slutförts. Nu kan du använda hello mrsdeploy paketet på din RClient tooconnect toohello Operationalization på kantnod och börja använda dess funktioner som [fjärrkörning](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) och [-webbtjänster](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). Om klustret har konfigurerats på ett virtuellt nätverk eller inte, måste du använda tooset in port vidarebefordra tunneltrafik via SSH-inloggning. hello följande avsnitt beskrivs hur tooset upp den här tunneln.

### <a name="rserver-cluster-on-virtual-network"></a>RServer-kluster i ett virtuellt nätverk

Kontrollera att du tillåter trafik genom porten 12800 toohello kantnoden. På så sätt kan du använda hello edge nod tooconnect toohello Operationalization funktionen.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Om hello `remoteLogin()` kan inte ansluta toohello kantnod, men du kan SSH toohello kantnod, måste du tooverify om hello regeln tooallow trafik på port 12800 har ställts in korrekt eller inte. Om du fortsätter tooface hello problemet kan kringgå du den genom att ställa in port vidarebefordra tunneltrafik via SSH. Instruktioner finns i följande avsnitt hello.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Inget RServer-kluster installerat i ett virtuellt nätverk

Om inget kluster har konfigurerats på vnet, eller om du har problem med anslutningen via vnet, kan du använda SSH-portvidarebefordran:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Du kan även konfigurera det på Putty.

![putty ssh-anslutning](./media/hdinsight-hadoop-r-server-get-started/putty.png)

När SSH-session är aktiv, vidarebefordras hello trafik från din dator port 12800 toohello kantnod port 12800 via SSH-session. Se till att du använder `127.0.0.1:12800` i metoden `remoteLogin()`. Loggas i toohello kant nodens operationalization via vidarebefordrade portar.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-tooscale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Hur tooscale Microsoft R Server Operationalization compute-noder på HDInsight arbetsnoder

### <a name="decommission-hello-worker-nodes"></a>Inaktivera hello worker noder

Microsoft R Server hanteras för närvarande inte via Yarn. Om hello arbetarnoder inte är inaktiverade, fungerar inte hello Yarn Resource Manager som förväntat, eftersom det är inte medveten om hello resurser tas upp av hello-servern. I ordning tooavoid i den här situationen rekommenderar vi inaktiverar hello arbetarnoder innan du skalar upp hello compute-noder.

Steg toodecommissioning arbetsnoderna:

* Logga in tooHDI klustret Ambari-konsolen och klicka på fliken ”värd”
* Välj arbetarnoder (toobe inaktiveras), klicka på ”Åtgärder” > ”valda värdar” > ”värd” > Klicka på ”Stäng av underhållsläge”. Till exempel har vi valt wn3 och wn4 toodecommission i hello följande bild.  

   ![inaktivera arbetsnoder](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **DataNodes** > klicka på **Inaktivera**
* Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **NodeManagers** > klicka på **Inaktivera**
* Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **DataNodes** > klicka på **Stoppa**
* Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **NodeManagers** > klicka på **Stoppa**
* Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **Värdar** > klicka på **Stop All Components (Stoppa alla komponenter)**
* Avmarkera hello arbetarnoder och välj hello huvudnoderna
* Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **Värdar** > **Restart All Components (Starta om alla komponenter)**

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Konfigurera beräkningsnoder på varje inaktiverad arbetsnod

1. SSH till varje inaktiverad arbetsnod.
2. Kör admin-verktyget med `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Ange ”1” tooselect alternativet ”konfigurera R Server för Operationalization”.
4. Ange alternativet ”c” tooselect ”C. ”Beräkningsnod”. Detta konfigurerar hello compute-nod på hello arbetsnoden.
5. Avsluta hello administrationsverktyget.

### <a name="add-compute-nodes-details-on-web-node"></a>Lägga till beräkningsnoder på webbnod

När alla inaktiverade arbetarnoder har konfigurerats beräkningsnod toorun, gå tillbaka på hello kantnod och lägger till IP-adresser för inaktiverade worker noder hello Microsoft R Server web nodens konfiguration:

* SSH i hello kantnod.
* Kör `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.
* Leta efter hello ”URI” avsnitt och Lägg till worker noden IP och port information.

    ![ta arbetsnoder ur drift, kommandorad](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).


## <a name="next-steps"></a>Nästa steg

Nu bör du förstå hur toocreate ett nytt HDInsight-kluster som innehåller hello R Server och hello grunderna i hello R-konsolen från en SSH-session. hello följande avsnitt innehåller information om andra sätt att hantera och arbeta med R Server på HDInsight:

* [Lägg till RStudio Server tooHDInsight (om inte installerad när klustret skapas)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Alternativ för beräkningskontexter för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Alternativ för Azure Storage för R Server på HDInsight](hdinsight-hadoop-r-server-storage.md)
