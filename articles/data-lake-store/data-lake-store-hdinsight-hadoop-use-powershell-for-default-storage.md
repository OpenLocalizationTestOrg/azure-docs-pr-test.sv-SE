---
title: "aaaCreate HDInsight-kluster med Data Lake Store som standardlagring med hjälp av PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell toocreate och använda HDInsight-kluster med Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/08/2017
ms.author: nitinme
ms.openlocfilehash: a5c0ad416da6ad9bd07204af2ebb6b7470916085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>Skapa HDInsight-kluster med Data Lake Store som standardlagring med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [Använd hello Azure-portalen](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Använd PowerShell (för standardlagring)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Använd PowerShell (för ytterligare lagringsutrymme)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Använd Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Lär dig hur toouse Azure PowerShell tooconfigure Azure HDInsight-kluster med Azure Data Lake Store, som standardlagring. Anvisningar om hur du skapar ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme finns [skapar ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme](data-lake-store-hdinsight-hadoop-use-powershell.md).

Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:

* hello alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som standardlagring är tillgänglig för HDInsight version 3.5 och 3,6.

* hello alternativet toocreate HDInsight-kluster med åtkomst till tooData Datasjölager eftersom standardlagring *inte tillgänglig* för HDInsight Premium-kluster.

tooconfigure HDInsight toowork med Data Lake Store med hjälp av PowerShell, följ instruktionerna för hello i följande fem hello-avsnitt.

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du kontrollera att du uppfyller följande krav hello:

* **En Azure-prenumeration**: gå för[hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 eller senare**: se [hur tooinstall och konfigurerar PowerShell](/powershell/azure/overview).
* **Windows Software Development Kit (SDK)**: tooinstall Windows SDK och gå för[laddar ned och verktyg för Windows 10](https://dev.windows.com/en-us/downloads). hello SDK är används toocreate säkerhetscertifikat.
* **Azure Active Directory-tjänstens huvudnamn**: den här självstudiekursen beskriver hur toocreate ett huvudnamn för tjänsten i Azure Active Directory (AD Azure). Dock toocreate ett huvudnamn för tjänsten, måste du vara administratör för Azure AD. Om du är administratör kan du hoppa över det här kravet och fortsätt med hello kursen.

    >[!NOTE]
    >Du kan skapa en tjänst huvudnamn endast om du är administratör för Azure AD. Azure AD-administratör skapa en tjänst huvudnamn innan du kan skapa ett HDInsight-kluster med Data Lake Store. hello tjänstens huvudnamn måste skapas med ett certifikat som beskrivs i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-store-account"></a>Skapa ett Data Lake Store-konto
toocreate ett Data Lake Store-konto hello följande:

1. Öppna ett PowerShell-fönster på skrivbordet och ange sedan hello fragmenten nedan. När du kan ange toosign i logga in som en hello prenumerationsadministratörer eller ägare. 

        # Sign in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Om du registerresursleverantören hello Data Lake Store och ett felmeddelande liknande för`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, prenumerationen kanske inte är godkända för Data Lake Store. tooenable din Azure-prenumeration för hello Data Lake Store public preview, följer du anvisningarna hello i [Kom igång med Azure Data Lake Store med hjälp av hello Azure-portalen](data-lake-store-get-started-portal.md).
    >

2. Ett Data Lake Store-konto är kopplat till en Azure-resursgrupp. Börja med att skapa en resursgrupp.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Du bör se utdata som liknar detta:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Skapa ett Data Lake Store-konto. hello konto namn som du anger måste innehålla endast små bokstäver och siffror.

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

4. Om du använder Data Lake Store som standardlagring måste toospecify rot sökvägen toowhich hello klusterspecifika filer kopieras när klustret skapas. toocreate en rotsökväg, vilket är **/kluster/hdiadlcluster** hello kodutdrag använda hello följande cmdlets:

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a>Konfigurera autentisering för rollbaserad åtkomst tooData Lake Store
Alla Azure-prenumerationer är associerade med en Azure AD-entitet. Användare och tjänster för att komma åt prenumerationsresurser med hjälp av hello Azure-portalen eller hello Azure Resource Manager API måste först autentisera med Azure AD. Komma åt tooAzure prenumerationer och tjänster genom att tilldela dem till lämpliga hello-rollen på en Azure-resurs. För tjänster identifierar ett huvudnamn för tjänsten hello tjänst i Azure AD.

Detta avsnitt visar hur toogrant ett program service, till exempel HDInsight, åtkomst tooan Azure-resurs (hello Data Lake Store-konto som du skapade tidigare). Det gör du genom att skapa en tjänst huvudnamn för programmet hello och tilldela roller tooit via PowerShell.

tooset in Active Directory-autentisering för Azure Data Lake utför hello uppgifter i hello följande två avsnitt.

### <a name="create-a-self-signed-certificate"></a>Skapa ett självsignerat certifikat
Kontrollera att du har [Windows SDK](https://dev.windows.com/en-us/downloads) installerad innan du fortsätter med hello stegen i det här avsnittet. Måste du också skapa en katalog som *C:\mycertdir*, där du skapar hello certifikat.

1. Gå toohello plats där du har installerat Windows SDK från hello PowerShell-fönster (vanligtvis *C:\Program Files (x86) \Windows Kits\10\bin\x86*) och använda hello [MakeCert] [ makecert] verktyget toocreate ett självsignerat certifikat och en privat nyckel. Använd hello följande kommandon:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Du kommer att tillfrågas tooenter hello lösenordet privata nyckeln. Efter hello-kommandot har körts, bör du se **CertFile.cer** och **mykey.pvk** i hello certifikat katalog som du angav.
2. Använd hello [Pvk2Pfx] [ pvk2pfx] verktyget tooconvert hello .pvk och .cer-filer som MakeCert skapade tooa .pfx-fil. Kör följande kommando hello:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    När du uppmanas ange hello privata nyckel lösenord som du angav tidigare. Hej värdet som du anger för hello **IO -** parameter är hello lösenord som är associerad med hello .pfx-fil. När hello-kommandot har slutförts, bör du också se en **CertFile.pfx** i hello certifikat katalog som du angav.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Skapa en Azure AD och ett huvudnamn för tjänsten
I det här avsnittet, skapa ett huvudnamn för tjänsten för Azure AD-program, tilldela en roll toohello tjänstens huvudnamn och autentisera sig som hello tjänstens huvudnamn genom att tillhandahålla ett certifikat. toocreate ett program i Azure AD, kör hello följande kommandon:

1. Klistra in följande cmdletar i PowerShell-konsolfönster för hello hello. Kontrollera att hello-värde som du anger för hello **- DisplayName** egenskapen är unika. Hej värden för **- webbsida** och **- IdentiferUris** är platshållarvärdena och kan inte verifieras.

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
2. Skapa ett huvudnamn för tjänsten med hjälp av hello program-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Bevilja hello service principal åtkomst toohello Data Lake Store rot och alla hello mappar i hello rotsökvägen som du angav tidigare. Använd hello följande cmdlets:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-hello-default-storage"></a>Skapa ett kluster i HDInsight Linux med Data Lake Store som hello standardlagring

I det här avsnittet skapar du ett HDInsight Hadoop Linux-kluster med Data Lake Store som hello standardlagring. Den här versionen hello HDInsight-kluster och Data Lake Store måste vara i hello samma plats.

1. Hämta hello prenumeration klient-ID och lagra den toouse senare.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Skapa hello HDInsight-kluster med hjälp av hello följande cmdlets:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Du bör se utdata som visar hello klusterinformation när hello cmdleten har slutförts.

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-data-lake-store"></a>Kör testjobb på hello HDInsight-kluster toouse Data Lake Store
När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på den tooensure att få åtkomst till Data Lake Store. toodo så kör ett exempel Hive-jobbet toocreate en tabell som använder hello exempeldata som redan finns i Data Lake Store på  *<cluster root>/example/data/sample.log*.

Du upprättar en anslutning för SSH (Secure Shell) till hello HDInsight Linux-kluster som du skapade i det här avsnittet och kör sedan en exempelfråga Hive.

* Om du använder en Windows-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Om du använder en Linux-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. När du har gjort hello anslutning startar du hello Hive-kommandoradsgränssnittet (CLI) med hjälp av hello följande kommando:

        hive
2. Använd hello CLI tooenter hello följande instruktioner toocreate en ny tabell med namnet **fordon** med hjälp av hello exempeldata i Data Lake Store:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Du bör se hello frågeresultatet på hello SSH-konsolen.

    >[!NOTE]
    >hello sökvägen toohello exempeldata i hello föregående CREATE TABLE-kommando är `adl:///example/data/`, där `adl:///` är hello kluster roten. Följande hello kluster roten som anges i den här självstudiekursen hello exempelvis hello-kommandot är `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Du kan använda hello kortare alternativ, eller så kan du ange hello fullständig sökväg toohello kluster roten.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>Åtkomst till Data Lake Store med hjälp av HDFS-kommandon
När du har konfigurerat hello HDInsight klustret toouse Data Lake Store kan använda du Hadoop Distributed File System (HDFS) shell-kommandon tooaccess hello store.

Du gör en SSH-anslutning till hello HDInsight Linux-kluster som du skapade i det här avsnittet och kör sedan hello HDFS-kommandon.

* Om du använder en Windows-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Om du använder en Linux-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

När du har gjort hello anslutning fil listan hello filer i Data Lake Store med hjälp av följande HDFS hello Systemkommando.

    hdfs dfs -ls adl:///

Du kan också använda hello `hdfs dfs -put` kommandot tooupload vissa filer tooData Lake Store och sedan använda `hdfs dfs -ls` tooverify hello filer har laddats.

## <a name="see-also"></a>Se även
* [Azure-portalen: skapa ett HDInsight-kluster toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
