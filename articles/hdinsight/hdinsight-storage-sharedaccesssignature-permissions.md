---
title: "aaaRestrict åtkomst med signaturer för delad åtkomst - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse signaturer för delad åtkomst toorestrict HDInsight åt toodata lagras i Azure storage BLOB."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: a34a2f8e52e47a15b09f09bc1fc67fc6159ec75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-shared-access-signatures-toorestrict-access-toodata-in-hdinsight"></a>Använd Azure Storage signaturer för delad åtkomst toorestrict åtkomst toodata i HDInsight

HDInsight har fullständig åtkomst toodata i hello Azure Storage-konton som är associerade med hello-kluster. Du kan använda signaturer för delad åtkomst på hello blob-behållaren toorestrict åtkomst toohello data. Till exempel tooprovide läsbehörighet toohello data. Delad åtkomst signaturer (SAS) är en funktion i Azure storage-konton som du kan använda toolimit åtkomst toodata. Till exempel tillhandahåller skrivskyddad åtkomst toodata.

> [!IMPORTANT]
> Överväg att använda domänanslutna HDInsight för en lösning som använder Apache Ranger. Mer information finns i hello [konfigurera domänanslutna HDInsight](hdinsight-domain-joined-configure.md) dokumentet.

> [!WARNING]
> HDInsight måste ha fullständig åtkomst toohello standard lagringen för hello klustret.

## <a name="requirements"></a>Krav

* En Azure-prenumeration
* C# eller Python. C#-kodexempel anges som ett Visual Studio-lösning.

  * Visual Studio måste vara version 2013 eller 2015 2017
  * Python måste vara version 2.7 eller högre

* Ett Linux-baserade HDInsight-kluster eller [Azure PowerShell] [ powershell] -om du har ett befintligt Linux-baserade kluster, kan du använda Ambari tooadd en signatur för delad åtkomst toohello klustret. Om inte, kan du använda Azure PowerShell toocreate ett kluster och lägga till en signatur för delad åtkomst när klustret skapas.

    > [!IMPORTANT]
    > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Hej exempel filer från [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Den här lagringsplatsen innehåller hello följande objekt:

  * Ett Visual Studio-projekt som kan skapa en lagringsbehållare, lagrade principer och SAS för användning med HDInsight
  * Python-skriptet som kan skapa en lagringsbehållare, lagrade principer och SAS för användning med HDInsight
  * Ett PowerShell-skript som kan skapa ett HDInsight-kluster och konfigurera den toouse hello SAS.

## <a name="shared-access-signatures"></a>Signaturer för delad åtkomst

Det finns två typer av signaturer för delad åtkomst:

* Ad hoc-: hello starttid, förfallotiden och behörigheter för hello SAS har angetts för hello SAS-URI.

* Lagras åtkomstprincip: en lagrade princip har definierats för en resurs-behållare, till exempel en blob-behållare. En princip kan vara används toomanage begränsningarna för en eller flera signaturer för delad åtkomst. När du kopplar en SAS med en lagrad till hello SAS ärver begränsningarna hello - hello starttid, förfallotiden samt behörigheter - har definierats för hello lagras åtkomstprincip.

hello skillnaden mellan hello två formulär är viktigt för ett key-scenario: återkallade certifikat. En SAS är en URL så att alla som erhåller hello SAS kan använda det, oavsett vem som har begärt det toobegin med. Om en SAS publiceras offentligt, kan den användas av alla i hälsningsmeddelande. En SAS som distribueras är giltig förrän något av följande händer:

1. hello förfallotiden anges på hello SAS har uppnåtts.

2. hello förfallotiden har angetts på hello lagras åtkomstprincip som refereras av hello SAS har uppnåtts. hello följande scenarier orsaka hello förfallotiden toobe uppnåtts:

    * hello tidsintervall har gått ut.
    * hello lagras åtkomstprincipen är ändrade toohave en förfallotiden i hello tidigare. Ändra hello förfallotiden är enkelriktade toorevoke hello SAS.

3. hello lagras åtkomstprincip som refereras av hello SAS tas bort, vilket är ett annat sätt toorevoke hello SAS. Om du återskapa hello lagras åtkomstprincip med samma namn, SAS-token för hello hello föregående principen gäller (om hello förfallotiden på hello SAS inte har passerats). Om du avser toorevoke hello SAS vara säker på att toouse ett annat namn om du återskapa hello åtkomstprincip med en förfallotiden i hello framtida.

4. Hej kontonyckel som har använt toocreate hello SAS genereras. Återskapar hello nyckeln gör alla program som använder hello tidigare viktiga toofail autentisering. Uppdatera alla komponenter toohello ny nyckel.

> [!IMPORTANT]
> En signatur för delad åtkomst URI är associerad med hello konto nyckel används toocreate hello signatur och hello associerad lagrade åtkomstprincip (eventuella). Om inga lagrade åtkomstprincip anges är hello endast sätt toorevoke en signatur för delad åtkomst toochange hello kontonyckel.

Vi rekommenderar att du alltid använder lagrade åtkomstprinciper. När du använder lagrade principer, kan du återkalla signaturer eller utöka hello utgångsdatum efter behov. hello stegen i det här dokumentet använder lagrade åtkomst principer toogenerate SAS.

Mer information om signaturer för delad åtkomst finns [förstå hello SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Skapa en princip för lagrade och SAS med hjälp av C\#

1. Öppna hello lösningen i Visual Studio.

2. I Solution Explorer högerklickar du på hello **SASToken** projektet och välj **egenskaper**.

3. Välj **inställningar** och lägga till värden för följande poster hello:

   * StorageConnectionString: hello anslutningssträngen för hello storage-konto som du vill toocreate lagrade politiska och SAS för. hello format bör vara `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` där `myaccount` är hello namnet på ditt lagringskonto och `mykey` är hello nyckel för hello storage-konto.

   * ContainerName: hello behållare i hello storage-konto som du vill toorestrict åtkomst till.

   * SASPolicyName: hello namnet toouse för hello lagras princip toocreate.

   * FileToUpload: hello sökvägen tooa fil som är överförda toohello behållare.

4. Kör hello-projektet. Information liknande toohello följande text som visas när hello SAS har skapats:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Spara princip hello SAS-token, lagringskontonamnet och behållarnamn. Dessa värden används när du associerar hello storage-konto med ditt HDInsight-kluster.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Skapa en princip för lagrade och SAS använder Python

1. Öppna hello SASToken.py filen och ändra hello följande värden:

   * principen\_name: hello namnet toouse för hello lagras princip toocreate.

   * lagring\_konto\_name: hello namnet på ditt lagringskonto.

   * lagring\_konto\_nyckel: hello nyckel för hello storage-konto.

   * lagring\_behållare\_name: hello behållare i hello storage-konto som du vill toorestrict åtkomst till.

   * exempel\_filen\_sökväg: hello sökvägen tooa fil som är överförda toohello behållare.

2. Kör skriptet hello. Hello SAS-token liknande toohello efter text när hello skriptet har slutförts visas:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Spara princip hello SAS-token, lagringskontonamnet och behållarnamn. Dessa värden används när du associerar hello storage-konto med ditt HDInsight-kluster.

## <a name="use-hello-sas-with-hdinsight"></a>Använda hello SAS med HDInsight

När du skapar ett HDInsight-kluster, måste du ange en primär storage-konto och du kan också ange ytterligare lagringskonton. Båda dessa metoder för att lägga till lagring kräver fullständig åtkomst toohello storage-konton och behållare som används.

toouse en signatur för delad åtkomst toolimit åtkomst tooa behållare, lägga till en anpassad post toohello **core-plats** konfigurationen för hello kluster.

* För **Windows-baserade** eller **Linux-baserade** HDInsight-kluster, du kan lägga till hello-post när klustret skapas med hjälp av PowerShell.
* För **Linux-baserade** HDInsight-kluster, ändra hello konfiguration efter att skapa ett kluster med Ambari.

### <a name="create-a-cluster-that-uses-hello-sas"></a>Skapa ett kluster som använder hello SAS

Ett exempel på hur du skapar ett HDInsight-kluster som använder hello SAS ingår i hello `CreateCluster` för hello-databasen. toouse det, Använd hello följande steg:

1. Öppna hello `CreateCluster\HDInsightSAS.ps1` filen i en textredigerare och ändra följande värden hello början av dokumentet hello hello.

    ```powershell
    # Replace 'mycluster' with hello name of hello cluster toobe created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with hello name of hello group toobe created
    $resourceGroupName = 'myresourcegroup'
    # Replace with hello Azure data center you want toohello cluster toolive in
    $location = 'North Europe'
    # Replace with hello name of hello default storage account toobe created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with hello name of hello SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with hello name of hello SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with hello SAS token generated earlier
    $SASToken = 'sastoken'
    # Set hello number of worker nodes in hello cluster
    $clusterSizeInNodes = 3
    ```

    Till exempel ändra `'mycluster'` toohello namn hello klustret som du vill toocreate. hello SAS-värden måste matcha hello värden från hello föregående steg när du skapar ett lagringskonto och SAS-token.

    När du har ändrat hello värden, spara hello-filen.

2. Öppna en ny Azure PowerShell-kommandotolk. Om du inte känner till Azure PowerShell eller inte har installerat den, se [installera och konfigurera Azure PowerShell][powershell].

1. Använd hello efter kommandot tooauthenticate tooyour Azure-prenumeration från hello-prompten:

    ```powershell
    Login-AzureRmAccount
    ```

    När du uppmanas logga in med hello-konto för din Azure-prenumeration.

    Om ditt konto är kopplat till flera Azure-prenumerationer måste du eventuellt toouse `Select-AzureRmSubscription` tooselect hello prenumeration som du vill toouse.

4. Hello-Kommandotolken, ändra kataloger toohello `CreateCluster` katalog som innehåller hello HDInsightSAS.ps1 fil. Använd följande kommandoskript toorun hello hello

    ```powershell
    .\HDInsightSAS.ps1
    ```

    När hello skript körs loggas utdata toohello PowerShell-Kommandotolken eftersom den skapar hello resurs grupp och storage-konton. Du kan ange tooenter hello HTTP användaren hello HDInsight-kluster. Det här kontot är används toosecure HTTP/s åtkomst toohello kluster.

    Om du skapar ett Linux-baserade kluster, efterfrågas ett SSH-konto användarnamn och lösenord. Det här kontot är används tooremotely logg i toohello kluster.

   > [!IMPORTANT]
   > När du uppmanas att ange hello HTTP/s eller SSH-användarnamn och lösenord, måste du ange ett lösenord som uppfyller följande kriterier hello:
   >
   > * Måste vara minst 10 tecken
   > * Måste innehålla minst en siffra
   > * Måste innehålla minst ett icke-alfanumeriska tecken
   > * Måste innehålla minst en versal eller gemen bokstav

Det tar en stund innan det här skriptet toocomplete vanligtvis cirka 15 minuter. När hello skriptet har slutförts utan fel har hello klustret skapats.

### <a name="use-hello-sas-with-an-existing-cluster"></a>Använda hello SAS med ett befintligt kluster

Om du har ett befintligt Linux-baserade kluster kan du lägga till hello SAS toohello **core-plats** konfiguration med hjälp av hello följande steg:

1. Öppna hello Ambari-webbgränssnittet för klustret. hello-adressen för den här sidan är https://YOURCLUSTERNAME.azurehdinsight.net. När du uppmanas att autentisera toohello kluster med hjälp av namn på serveradministratör hello (admin) och lösenord som du använde när du skapar hello-kluster.

2. Hello vänster sida av hello Ambari-webbgränssnittet, Välj **HDFS** och välj sedan hello **konfigurationerna** fliken hello mitten av hello-sidan.

3. Välj hello **Avancerat** , och Bläddra tills du hittar hello **anpassad core-plats** avsnitt.

4. Expandera hello **anpassad core-plats** avsnittet, och sedan rullar toohello slut och välj hello **Lägg till egenskap...**  länk. Använd hello följande värden för hello **nyckeln** och **värdet** fält:

   * **Nyckeln**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Värdet**: hello SAS som returneras av hello C# eller Python-program som du tidigare körde

     Ersätt **CONTAINERNAME** med hello behållarens namn används med hello C# eller SAS-program. Ersätt **STORAGEACCOUNTNAME** med hello behållarens kontonamn som du använde.

5. Klicka på hello **Lägg till** knappen toosave detta nyckel och värde och klicka sedan på hello **spara** knappen toosave hello konfigurationsändringar. När du uppmanas, Lägg till en beskrivning av hello ändring (”lägger till SAS-lagringsåtkomst” till exempel) och klicka sedan på **spara**.

    Klicka på **OK** när hello ändringar har utförts.

   > [!IMPORTANT]
   > Du måste starta om flera tjänster innan hello ändringen börjar gälla.

6. I hello Ambari-webbgränssnittet, väljer **HDFS** hello listan hello vänster och välj sedan **starta om alla** från hello **tjänståtgärder** listrutan på hello rätt. När du uppmanas, Välj **aktivera underhållsläge** och sedan väljer __Conform starta om alla ”.

    Upprepa processen för MapReduce2 och YARN.

7. När hello tjänsterna har startats om, välja respektive och inaktivera underhållsläge från hello **tjänståtgärder** listrutan.

## <a name="test-restricted-access"></a>Testa begränsad åtkomst

tooverify som du har begränsad åtkomst, användning hello följande metoder:

* För **Windows-baserade** HDInsight-kluster, använda Fjärrskrivbord tooconnect toohello kluster. Mer information finns i [ansluta tooHDInsight med](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    När du är ansluten, Använd hello **Hadoop kommandoradsverktyget** ikon på hello skrivbord tooopen en kommandotolk.

* För **Linux-baserade** HDInsight-kluster använder SSH tooconnect toohello kluster. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

När du är ansluten toohello klustret Använd hello följande steg tooverify som du kan endast Läs- och objekt för hello SAS storage-konto:

1. toolist hello innehållet i hello behållare, Använd hello följande kommando från hello prompten: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Ersätt **SASCONTAINER** med hello namnet hello-behållare som skapats för hello SAS storage-konto. Ersätt **SASACCOUNTNAME** med hello namnet hello storage-konto som används för hello SAS.

    hello listan innehåller hello-fil som överförs när hello-behållaren och SAS skapades.

2. Använd hello följande kommando tooverify att du kan läsa hello innehållet i hello-fil. Ersätt hello **SASCONTAINER** och **SASACCOUNTNAME** som hello föregående steg. Ersätt **FILENAME** med hello namnet på hello-fil som visas i föregående hello-kommando:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Det här kommandot visar hello innehållet i hello-fil.

3. Använd hello följande kommando lokalt filsystem för toodownload hello för filen toohello:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Det här kommandot hämtningar hello tooa lokal fil med namnet **testfile.txt**.

4. Använd hello följande kommando tooupload hello lokal fil tooa ny fil med namnet **testupload.txt** på hello SAS-lagring:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Du får ett meddelande liknande toohello följande text:

        put: java.io.IOException

    Detta fel uppstår eftersom hello-lagringsplatsen är skrivskyddade + endast lista över tillåtna. Använd hello följande kommando tooput hello data på hello standardlagring för hello-kluster, vilket är skrivbar:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Den här tiden kan ska hello åtgärden slutföras.

## <a name="troubleshooting"></a>Felsökning

### <a name="a-task-was-canceled"></a>En uppgift har avbrutits

**Symptom**: när du skapar ett kluster med hello PowerShell-skript får hello följande felmeddelande:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Orsak**: det här felet kan inträffa om du använder ett lösenord för hello admin/http-användare för hello kluster, eller (för Linux-baserade kluster) hello SSH-användare.

**Lösning**: Använd ett lösenord som uppfyller följande kriterier hello:

* Måste vara minst 10 tecken
* Måste innehålla minst en siffra
* Måste innehålla minst ett icke-alfanumeriska tecken
* Måste innehålla minst en versal eller gemen bokstav

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur tooadd lagring med begränsad åtkomst tooyour HDInsight-kluster Läs andra sätt toowork med data i klustret:

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
