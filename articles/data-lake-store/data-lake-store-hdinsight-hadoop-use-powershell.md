---
Rubrik: aaa ”PowerShell: Azure HDInsight-kluster med Data Lake Store som tilläggslagring | Microsoft Docs ”services: data lake store, hdinsight dokumentationcenter: '' författare: nitinme manager: jhubbard editor: cgronlun

MS.AssetID: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: data lake store ms.devlang: na ms.topic: artikel ms.tgt_pltfrm: na ms.workload: stordata ms.date: 06/08/2017 ms.author: nitinme

---
# <a name="use-azure-powershell-toocreate-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a>Använda Azure PowerShell toocreate ett HDInsight-kluster med Data Lake Store (som ytterligare lagringsutrymme)
> [!div class="op_single_selector"]
> * [Använda portalen](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Med hjälp av PowerShell (för standardlagring)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Med hjälp av PowerShell (för ytterligare lagringsutrymme)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Med Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Lär dig hur toouse Azure PowerShell tooconfigure ett HDInsight-kluster med Azure Data Lake Store **som ytterligare lagringsutrymme**. Anvisningar för hur toocreate ett HDInsight-kluster med Azure Data Lake Store som standardlagring finns [skapar ett HDInsight-kluster med Data Lake Store som standardlagring](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> Om du ska toouse Azure Data Lake Store som ytterligare lagringsutrymme för HDInsight-kluster, rekommenderar vi att du gör detta när du skapar klustret hello som beskrivs i den här artikeln. Lägga till Azure Data Lake Store som ytterligare lagringsutrymme tooan är befintligt HDInsight-kluster en komplicerad process och felbenägna tooerrors.
>

Data Lake Store kan användas som en standardlagring eller ytterligare storage-konto för stöds klustertyper. När du använder Data Lake Store som ytterligare lagringsutrymme hello standardkontot för lagring för hello kluster kommer fortfarande att Azure Storage BLOB (WASB) och hello kluster-relaterade filer (till exempel loggar osv.) skrivs fortfarande toohello standardlagring medan hello data som du vill tooprocess kan lagras i ett Data Lake Store-konto. Med Data Lake Store som ett ytterligare storage-konto inte påverkar prestanda eller hello möjlighet tooread/skrivning toohello lagring från hello kluster.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Med hjälp av Data Lake Store för HDInsight klusterlagring

Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:

* Alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som ytterligare lagring är tillgänglig för HDInsight version 3.2, 3.4, 3.5 och 3,6.

Konfigurera HDInsight omfattar toowork med Data Lake Store med hjälp av PowerShell hello följande steg:

* Skapa ett Azure Data Lake Store
* Konfigurera autentisering för rollbaserad åtkomst tooData Lake Store
* Skapa HDInsight-kluster med autentisering tooData Lake Store
* Köra ett testjobb på hello kluster

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Installera Azure PowerShell 1.0 eller senare**. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* **Windows SDK**. Du kan installera den från [här](https://dev.windows.com/en-us/downloads). Du kan använda den här toocreate säkerhetscertifikat.
* **Azure Active Directory Service Principal**. Stegen i den här kursen ger instruktioner för hur toocreate ett huvudnamn för tjänsten i Azure AD. Du måste dock vara en kan toocreate för Azure AD-administratör toobe på ett huvudnamn för tjänsten. Om du är administratör för Azure AD kan du hoppa över det här kravet och fortsätt med hello kursen.

    **Om du inte är en Azure AD-administratör**, kommer du inte att kunna tooperform hello steg krävs toocreate ett huvudnamn för tjänsten. I sådana fall måste din Azure AD-administratör först skapa ett huvudnamn för tjänsten innan du kan skapa ett HDInsight-kluster med Data Lake Store. Dessutom hello tjänstens huvudnamn måste skapas med hjälp av ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-azure-data-lake-store"></a>Skapa ett Azure Data Lake Store
Följ dessa steg toocreate ett Data Lake Store.

1. Öppna ett nytt Azure PowerShell-fönster på skrivbordet och ange hello följande kodavsnitt. När du tillfrågas toolog i, se till att logga in som en Hej administratör/prenumerationsägaren:

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Om du får ett felmeddelande för`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` när registrering hello Data Lake Store-resursprovidern, är det möjligt att din prenumeration inte är godkända för Azure Data Lake Store. Kontrollera att du aktiverar din Azure-prenumeration för Data Lake Store public preview genom att följa dessa [instruktioner](data-lake-store-get-started-portal.md).
   >
   >
2. Ett Azure Data Lake Store-konto är kopplat till en resursgrupp i Azure. Börja med att skapa en Azure-resursgrupp.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Du bör se utdata som liknar detta:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Skapa ett Azure Data Lake Store-konto. hello konto namn som du anger får bara innehålla gemena bokstäver och siffror.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    Du bör se utdata som liknar hello följande:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

5. Ladda upp vissa exempel data tooAzure Data Lake. Vi använder informationen senare i den här artikeln tooverify att hello data är tillgänglig från ett HDInsight-kluster. Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path toodata>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a>Konfigurera autentisering för rollbaserad åtkomst tooData Lake Store
Varje Azure-prenumeration är associerad med ett Azure Active Directory. Användare och tjänster som har åtkomst till resurser hello prenumeration med hjälp av hello klassiska Azure-portalen eller Azure Resource Manager API måste först autentisera med den Azure Active Directory. Komma åt tooAzure prenumerationer och tjänster genom att tilldela dem till lämpliga hello-rollen på en Azure-resurs.  För tjänster identifierar en tjänstens huvudnamn hello tjänst i hello Azure Active Directory (AAD). Detta avsnitt visar hur toogrant ett program tjänsten som HDInsight åtkomst tooan Azure-resurs (hello Azure Data Lake Store-konto som du skapade tidigare) genom att skapa ett huvudnamn för tjänsten för hello program och tilldela roller toothat via Azure PowerShell.

tooset in Active Directory-autentisering för Azure Data Lake måste du utföra följande uppgifter hello.

* Skapa ett självsignerat certifikat
* Skapa ett program i Azure Active Directory och ett huvudnamn för tjänsten

### <a name="create-a-self-signed-certificate"></a>Skapa ett självsignerat certifikat
Kontrollera att du har [Windows SDK](https://dev.windows.com/en-us/downloads) installerad innan du fortsätter med hello stegen i det här avsnittet. Måste du också skapa en katalog som **C:\mycertdir**, där hello certifikat kommer att skapas.

1. Navigera toohello plats där du har installerat Windows SDK från hello PowerShell-fönster (vanligtvis `C:\Program Files (x86)\Windows Kits\10\bin\x86` och använda hello [MakeCert] [ makecert] verktyget toocreate ett självsignerat certifikat och en privat nyckel. Använd följande kommandon hello.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Du kommer att tillfrågas tooenter hello lösenordet privata nyckeln. När hello har körs kommandot, bör du se en **CertFile.cer** och **mykey.pvk** i hello certifikat katalog du angett.
2. Använd hello [Pvk2Pfx] [ pvk2pfx] verktyget tooconvert hello .pvk och .cer-filer som MakeCert skapade tooa .pfx-fil. Kör följande kommando hello.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    När du uppmanas ange hello privat lösenord du angav tidigare. Hej värdet som du anger för hello **IO -** parameter är hello lösenord som är associerad med hello .pfx-fil. Du bör också se en CertFile.pfx i hello certifikat katalog du angett efter hello-kommandot har slutförts.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Skapa ett Azure Active Directory och ett huvudnamn för tjänsten
I det här avsnittet, utföra hello steg toocreate ett huvudnamn för tjänsten för ett Azure Active Directory-program, tilldela en roll toohello tjänstens huvudnamn och autentisera sig som hello tjänstens huvudnamn genom att tillhandahålla ett certifikat. Kör följande kommandon toocreate hello ett program i Azure Active Directory.

1. Klistra in följande cmdletar i PowerShell-konsolfönster för hello hello. Kontrollera att hello-värdet som du anger för hello **- DisplayName** egenskapen är unika. Dessutom hello värden för **- webbsida** och **- IdentiferUris** är platshållarvärdena och kan inte verifieras.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter hello password" # This is hello password you specified for hello .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Skapa ett huvudnamn för tjänsten med hello program-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Bevilja hello service principal access toohello Data Lake Store-mappen och hello-fil som du kommer åt från hello HDInsight-kluster. hello kodfragmentet nedan innehåller åtkomst toohello rot hello Data Lake Store-konto (dit du kopierade hello exempeldatafil) och hello själva filen.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a>Skapa ett kluster i HDInsight Linux med Data Lake Store som ytterligare lagringsutrymme

I det här avsnittet skapar vi ett HDInsight Hadoop Linux-kluster med Data Lake Store som ytterligare lagringsutrymme. Hej HDInsight-kluster och hello Data Lake Store för den här versionen måste vara i hello samma plats.

1. Börja med att hämta hello prenumeration klient-ID. Du behöver som senare.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. Den här versionen kan för Hadoop-kluster Data Lake Store endast användas som ett ytterligare lagringsutrymme för hello klustret. hello standardlagring kommer fortfarande att hello Azure storage-blobbar (WASB). Så måste vi först skapa hello storage-konto och lagringsbehållare som krävs för hello klustret.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Skapa hello HDInsight-kluster. Använd hello följande cmdlet: ar.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have hello same name for hello cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Du bör se utdata visar en lista över hello klusterinformation efter hello cmdleten har slutförts.


## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a>Kör testjobb på hello HDInsight-kluster toouse hello Data Lake Store
När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på hello klustret tootest som hello HDInsight klustret kan komma åt Data Lake Store. toodo så vi ska köra en Hive-jobb som skapar en tabell med hello exempeldata som du överfört tidigare tooyour Data Lake Store.

I det här avsnittet kommer du att SSH till hello HDInsight Linux klustret du skapade och kör hello en exempelfråga Hive.

* Om du använder en Windows-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Om du använder en Linux-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. När du är ansluten, starta hello Hive CLI med hello följande kommando:

        hive
2. Med hjälp av hello CLI, ange hello följande instruktioner toocreate en ny tabell med namnet **fordon** med hello exempeldata i hello Data Lake Store:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Du bör se en utdata liknande toohello följande:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-store-using-hdfs-commands"></a>Åtkomst till Data Lake Store med hjälp av HDFS-kommandon
När du har konfigurerat hello HDInsight klustret toouse Data Lake Store kan använda du hello HDFS shell-kommandon tooaccess hello store.

I det här avsnittet kommer du att SSH till hello HDInsight Linux klustret du skapade och kör hello HDFS-kommandon.

* Om du använder en Windows-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Om du använder en Linux-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

När du är ansluten, Använd hello följande HDFS filesystem kommandot toolist hello filer i hello Data Lake Store.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Du bör nu se hello-fil som du överfört tidigare toohello Data Lake Store.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Du kan också använda hello `hdfs dfs -put` kommandot tooupload vissa filer toohello Data Lake Store och sedan använda `hdfs dfs -ls` tooverify hello filer har laddats.

## <a name="see-also"></a>Se även
* [Portalen: Skapa ett HDInsight-kluster toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
