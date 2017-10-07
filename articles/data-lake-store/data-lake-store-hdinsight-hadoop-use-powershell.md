---
<span data-ttu-id="cddcf-101">Rubrik: aaa ”PowerShell: Azure HDInsight-kluster med Data Lake Store som tilläggslagring | Microsoft Docs ”services: data lake store, hdinsight dokumentationcenter: '' författare: nitinme manager: jhubbard editor: cgronlun</span><span class="sxs-lookup"><span data-stu-id="cddcf-101">title: aaa"PowerShell: Azure HDInsight cluster with Data Lake Store as add-on storage | Microsoft Docs" services: data-lake-store,hdinsight documentationcenter: '' author: nitinme manager: jhubbard editor: cgronlun</span></span>

<span data-ttu-id="cddcf-102">MS.AssetID: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: data lake store ms.devlang: na ms.topic: artikel ms.tgt_pltfrm: na ms.workload: stordata ms.date: 06/08/2017 ms.author: nitinme</span><span class="sxs-lookup"><span data-stu-id="cddcf-102">ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: data-lake-store ms.devlang: na ms.topic: article ms.tgt_pltfrm: na ms.workload: big-data ms.date: 06/08/2017 ms.author: nitinme</span></span>

---
# <a name="use-azure-powershell-toocreate-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="cddcf-103">Använda Azure PowerShell toocreate ett HDInsight-kluster med Data Lake Store (som ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="cddcf-103">Use Azure PowerShell toocreate an HDInsight cluster with Data Lake Store (as additional storage)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cddcf-104">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="cddcf-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="cddcf-105">Med hjälp av PowerShell (för standardlagring)</span><span class="sxs-lookup"><span data-stu-id="cddcf-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="cddcf-106">Med hjälp av PowerShell (för ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="cddcf-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="cddcf-107">Med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cddcf-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="cddcf-108">Lär dig hur toouse Azure PowerShell tooconfigure ett HDInsight-kluster med Azure Data Lake Store **som ytterligare lagringsutrymme**.</span><span class="sxs-lookup"><span data-stu-id="cddcf-108">Learn how toouse Azure PowerShell tooconfigure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span> <span data-ttu-id="cddcf-109">Anvisningar för hur toocreate ett HDInsight-kluster med Azure Data Lake Store som standardlagring finns [skapar ett HDInsight-kluster med Data Lake Store som standardlagring](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span><span class="sxs-lookup"><span data-stu-id="cddcf-109">For instructions on how toocreate an HDInsight cluster with Azure Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store as default storage](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cddcf-110">Om du ska toouse Azure Data Lake Store som ytterligare lagringsutrymme för HDInsight-kluster, rekommenderar vi att du gör detta när du skapar klustret hello som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="cddcf-110">If you are going toouse Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create hello cluster as described in this article.</span></span> <span data-ttu-id="cddcf-111">Lägga till Azure Data Lake Store som ytterligare lagringsutrymme tooan är befintligt HDInsight-kluster en komplicerad process och felbenägna tooerrors.</span><span class="sxs-lookup"><span data-stu-id="cddcf-111">Adding Azure Data Lake Store as additional storage tooan existing HDInsight cluster is a complicated process and prone tooerrors.</span></span>
>

<span data-ttu-id="cddcf-112">Data Lake Store kan användas som en standardlagring eller ytterligare storage-konto för stöds klustertyper.</span><span class="sxs-lookup"><span data-stu-id="cddcf-112">For supported cluster types, Data Lake Store can be used as a default storage or additional storage account.</span></span> <span data-ttu-id="cddcf-113">När du använder Data Lake Store som ytterligare lagringsutrymme hello standardkontot för lagring för hello kluster kommer fortfarande att Azure Storage BLOB (WASB) och hello kluster-relaterade filer (till exempel loggar osv.) skrivs fortfarande toohello standardlagring medan hello data som du vill tooprocess kan lagras i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="cddcf-113">When Data Lake Store is used as additional storage, hello default storage account for hello clusters will still be Azure Storage Blobs (WASB) and hello cluster-related files (such as logs, etc.) are still written toohello default storage, while hello data that you want tooprocess can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="cddcf-114">Med Data Lake Store som ett ytterligare storage-konto inte påverkar prestanda eller hello möjlighet tooread/skrivning toohello lagring från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="cddcf-114">Using Data Lake Store as an additional storage account does not impact performance or hello ability tooread/write toohello storage from hello cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="cddcf-115">Med hjälp av Data Lake Store för HDInsight klusterlagring</span><span class="sxs-lookup"><span data-stu-id="cddcf-115">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="cddcf-116">Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="cddcf-116">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="cddcf-117">Alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som ytterligare lagring är tillgänglig för HDInsight version 3.2, 3.4, 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="cddcf-117">Option toocreate HDInsight clusters with access tooData Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="cddcf-118">Konfigurera HDInsight omfattar toowork med Data Lake Store med hjälp av PowerShell hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cddcf-118">Configuring HDInsight toowork with Data Lake Store using PowerShell involves hello following steps:</span></span>

* <span data-ttu-id="cddcf-119">Skapa ett Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-119">Create an Azure Data Lake Store</span></span>
* <span data-ttu-id="cddcf-120">Konfigurera autentisering för rollbaserad åtkomst tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-120">Set up authentication for role-based access tooData Lake Store</span></span>
* <span data-ttu-id="cddcf-121">Skapa HDInsight-kluster med autentisering tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-121">Create HDInsight cluster with authentication tooData Lake Store</span></span>
* <span data-ttu-id="cddcf-122">Köra ett testjobb på hello kluster</span><span class="sxs-lookup"><span data-stu-id="cddcf-122">Run a test job on hello cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cddcf-123">Krav</span><span class="sxs-lookup"><span data-stu-id="cddcf-123">Prerequisites</span></span>
<span data-ttu-id="cddcf-124">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="cddcf-124">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="cddcf-125">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="cddcf-125">**An Azure subscription**.</span></span> <span data-ttu-id="cddcf-126">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cddcf-126">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="cddcf-127">**Installera Azure PowerShell 1.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="cddcf-127">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="cddcf-128">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cddcf-128">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="cddcf-129">**Windows SDK**.</span><span class="sxs-lookup"><span data-stu-id="cddcf-129">**Windows SDK**.</span></span> <span data-ttu-id="cddcf-130">Du kan installera den från [här](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="cddcf-130">You can install it from [here](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="cddcf-131">Du kan använda den här toocreate säkerhetscertifikat.</span><span class="sxs-lookup"><span data-stu-id="cddcf-131">You use this toocreate a security certificate.</span></span>
* <span data-ttu-id="cddcf-132">**Azure Active Directory Service Principal**.</span><span class="sxs-lookup"><span data-stu-id="cddcf-132">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="cddcf-133">Stegen i den här kursen ger instruktioner för hur toocreate ett huvudnamn för tjänsten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cddcf-133">Steps in this tutorial provide instructions on how toocreate a service principal in Azure AD.</span></span> <span data-ttu-id="cddcf-134">Du måste dock vara en kan toocreate för Azure AD-administratör toobe på ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cddcf-134">However, you must be an Azure AD administrator toobe able toocreate a service principal.</span></span> <span data-ttu-id="cddcf-135">Om du är administratör för Azure AD kan du hoppa över det här kravet och fortsätt med hello kursen.</span><span class="sxs-lookup"><span data-stu-id="cddcf-135">If you are an Azure AD administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    <span data-ttu-id="cddcf-136">**Om du inte är en Azure AD-administratör**, kommer du inte att kunna tooperform hello steg krävs toocreate ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cddcf-136">**If you are not an Azure AD administrator**, you will not be able tooperform hello steps required toocreate a service principal.</span></span> <span data-ttu-id="cddcf-137">I sådana fall måste din Azure AD-administratör först skapa ett huvudnamn för tjänsten innan du kan skapa ett HDInsight-kluster med Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-137">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="cddcf-138">Dessutom hello tjänstens huvudnamn måste skapas med hjälp av ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="cddcf-138">Also, hello service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="cddcf-139">Skapa ett Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-139">Create an Azure Data Lake Store</span></span>
<span data-ttu-id="cddcf-140">Följ dessa steg toocreate ett Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-140">Follow these steps toocreate a Data Lake Store.</span></span>

1. <span data-ttu-id="cddcf-141">Öppna ett nytt Azure PowerShell-fönster på skrivbordet och ange hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="cddcf-141">From your desktop, open a new Azure PowerShell window, and enter hello following snippet.</span></span> <span data-ttu-id="cddcf-142">När du tillfrågas toolog i, se till att logga in som en Hej administratör/prenumerationsägaren:</span><span class="sxs-lookup"><span data-stu-id="cddcf-142">When prompted toolog in, make sure you log in as one of hello subscription administrator/owner:</span></span>

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > <span data-ttu-id="cddcf-143">Om du får ett felmeddelande för`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` när registrering hello Data Lake Store-resursprovidern, är det möjligt att din prenumeration inte är godkända för Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-143">If you receive an error similar too`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` when registering hello Data Lake Store resource provider, it is possible that your subscription is not whitelisted for Azure Data Lake Store.</span></span> <span data-ttu-id="cddcf-144">Kontrollera att du aktiverar din Azure-prenumeration för Data Lake Store public preview genom att följa dessa [instruktioner](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cddcf-144">Make sure you enable your Azure subscription for Data Lake Store public preview by following these [instructions](data-lake-store-get-started-portal.md).</span></span>
   >
   >
2. <span data-ttu-id="cddcf-145">Ett Azure Data Lake Store-konto är kopplat till en resursgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="cddcf-145">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="cddcf-146">Börja med att skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="cddcf-146">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="cddcf-147">Du bör se utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="cddcf-147">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="cddcf-148">Skapa ett Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="cddcf-148">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="cddcf-149">hello konto namn som du anger får bara innehålla gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="cddcf-149">hello account name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="cddcf-150">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="cddcf-150">You should see an output like hello following:</span></span>

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

5. <span data-ttu-id="cddcf-151">Ladda upp vissa exempel data tooAzure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="cddcf-151">Upload some sample data tooAzure Data Lake.</span></span> <span data-ttu-id="cddcf-152">Vi använder informationen senare i den här artikeln tooverify att hello data är tillgänglig från ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cddcf-152">We'll use this later in this article tooverify that hello data is accessible from an HDInsight cluster.</span></span> <span data-ttu-id="cddcf-153">Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="cddcf-153">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path toodata>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a><span data-ttu-id="cddcf-154">Konfigurera autentisering för rollbaserad åtkomst tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-154">Set up authentication for role-based access tooData Lake Store</span></span>
<span data-ttu-id="cddcf-155">Varje Azure-prenumeration är associerad med ett Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cddcf-155">Every Azure subscription is associated with an Azure Active Directory.</span></span> <span data-ttu-id="cddcf-156">Användare och tjänster som har åtkomst till resurser hello prenumeration med hjälp av hello klassiska Azure-portalen eller Azure Resource Manager API måste först autentisera med den Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cddcf-156">Users and services that access resources of hello subscription using hello Azure Classic Portal or Azure Resource Manager API must first authenticate with that Azure Active Directory.</span></span> <span data-ttu-id="cddcf-157">Komma åt tooAzure prenumerationer och tjänster genom att tilldela dem till lämpliga hello-rollen på en Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="cddcf-157">Access is granted tooAzure subscriptions and services by assigning them hello appropriate role on an Azure resource.</span></span>  <span data-ttu-id="cddcf-158">För tjänster identifierar en tjänstens huvudnamn hello tjänst i hello Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="cddcf-158">For services, a service principal identifies hello service in hello Azure Active Directory (AAD).</span></span> <span data-ttu-id="cddcf-159">Detta avsnitt visar hur toogrant ett program tjänsten som HDInsight åtkomst tooan Azure-resurs (hello Azure Data Lake Store-konto som du skapade tidigare) genom att skapa ett huvudnamn för tjänsten för hello program och tilldela roller toothat via Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cddcf-159">This section illustrates how toogrant an application service, like HDInsight, access tooan Azure resource (hello Azure Data Lake Store account you created earlier) by creating a service principal for hello application and assigning roles toothat via Azure PowerShell.</span></span>

<span data-ttu-id="cddcf-160">tooset in Active Directory-autentisering för Azure Data Lake måste du utföra följande uppgifter hello.</span><span class="sxs-lookup"><span data-stu-id="cddcf-160">tooset up Active Directory authentication for Azure Data Lake, you must perform hello following tasks.</span></span>

* <span data-ttu-id="cddcf-161">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="cddcf-161">Create a self-signed certificate</span></span>
* <span data-ttu-id="cddcf-162">Skapa ett program i Azure Active Directory och ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="cddcf-162">Create an application in Azure Active Directory and a Service Principal</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="cddcf-163">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="cddcf-163">Create a self-signed certificate</span></span>
<span data-ttu-id="cddcf-164">Kontrollera att du har [Windows SDK](https://dev.windows.com/en-us/downloads) installerad innan du fortsätter med hello stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="cddcf-164">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with hello steps in this section.</span></span> <span data-ttu-id="cddcf-165">Måste du också skapa en katalog som **C:\mycertdir**, där hello certifikat kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="cddcf-165">You must have also created a directory, such as **C:\mycertdir**, where hello certificate will be created.</span></span>

1. <span data-ttu-id="cddcf-166">Navigera toohello plats där du har installerat Windows SDK från hello PowerShell-fönster (vanligtvis `C:\Program Files (x86)\Windows Kits\10\bin\x86` och använda hello [MakeCert] [ makecert] verktyget toocreate ett självsignerat certifikat och en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="cddcf-166">From hello PowerShell window, navigate toohello location where you installed Windows SDK (typically, `C:\Program Files (x86)\Windows Kits\10\bin\x86` and use hello [MakeCert][makecert] utility toocreate a self-signed certificate and a private key.</span></span> <span data-ttu-id="cddcf-167">Använd följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="cddcf-167">Use hello following commands.</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="cddcf-168">Du kommer att tillfrågas tooenter hello lösenordet privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="cddcf-168">You will be prompted tooenter hello private key password.</span></span> <span data-ttu-id="cddcf-169">När hello har körs kommandot, bör du se en **CertFile.cer** och **mykey.pvk** i hello certifikat katalog du angett.</span><span class="sxs-lookup"><span data-stu-id="cddcf-169">After hello command successfully executes, you should see a **CertFile.cer** and **mykey.pvk** in hello certificate directory you specified.</span></span>
2. <span data-ttu-id="cddcf-170">Använd hello [Pvk2Pfx] [ pvk2pfx] verktyget tooconvert hello .pvk och .cer-filer som MakeCert skapade tooa .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="cddcf-170">Use hello [Pvk2Pfx][pvk2pfx] utility tooconvert hello .pvk and .cer files that MakeCert created tooa .pfx file.</span></span> <span data-ttu-id="cddcf-171">Kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="cddcf-171">Run hello following command.</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="cddcf-172">När du uppmanas ange hello privat lösenord du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="cddcf-172">When prompted enter hello private key password you specified earlier.</span></span> <span data-ttu-id="cddcf-173">Hej värdet som du anger för hello **IO -** parameter är hello lösenord som är associerad med hello .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="cddcf-173">hello value you specify for hello **-po** parameter is hello password that is associated with hello .pfx file.</span></span> <span data-ttu-id="cddcf-174">Du bör också se en CertFile.pfx i hello certifikat katalog du angett efter hello-kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="cddcf-174">After hello command successfully completes, you should also see a CertFile.pfx in hello certificate directory you specified.</span></span>

### <a name="create-an-azure-active-directory-and-a-service-principal"></a><span data-ttu-id="cddcf-175">Skapa ett Azure Active Directory och ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="cddcf-175">Create an Azure Active Directory and a service principal</span></span>
<span data-ttu-id="cddcf-176">I det här avsnittet, utföra hello steg toocreate ett huvudnamn för tjänsten för ett Azure Active Directory-program, tilldela en roll toohello tjänstens huvudnamn och autentisera sig som hello tjänstens huvudnamn genom att tillhandahålla ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="cddcf-176">In this section, you perform hello steps toocreate a service principal for an Azure Active Directory application, assign a role toohello service principal, and authenticate as hello service principal by providing a certificate.</span></span> <span data-ttu-id="cddcf-177">Kör följande kommandon toocreate hello ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cddcf-177">Run hello following commands toocreate an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="cddcf-178">Klistra in följande cmdletar i PowerShell-konsolfönster för hello hello.</span><span class="sxs-lookup"><span data-stu-id="cddcf-178">Paste hello following cmdlets in hello PowerShell console window.</span></span> <span data-ttu-id="cddcf-179">Kontrollera att hello-värdet som du anger för hello **- DisplayName** egenskapen är unika.</span><span class="sxs-lookup"><span data-stu-id="cddcf-179">Make sure hello value you specify for hello **-DisplayName** property is unique.</span></span> <span data-ttu-id="cddcf-180">Dessutom hello värden för **- webbsida** och **- IdentiferUris** är platshållarvärdena och kan inte verifieras.</span><span class="sxs-lookup"><span data-stu-id="cddcf-180">Also, hello values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

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
2. <span data-ttu-id="cddcf-181">Skapa ett huvudnamn för tjänsten med hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="cddcf-181">Create a service principal using hello application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="cddcf-182">Bevilja hello service principal access toohello Data Lake Store-mappen och hello-fil som du kommer åt från hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cddcf-182">Grant hello service principal access toohello Data Lake Store folder and hello file that you will access from hello HDInsight cluster.</span></span> <span data-ttu-id="cddcf-183">hello kodfragmentet nedan innehåller åtkomst toohello rot hello Data Lake Store-konto (dit du kopierade hello exempeldatafil) och hello själva filen.</span><span class="sxs-lookup"><span data-stu-id="cddcf-183">hello snippet below provides access toohello root of hello Data Lake Store account (where you copied hello sample data file), and hello file itself.</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="cddcf-184">Skapa ett kluster i HDInsight Linux med Data Lake Store som ytterligare lagringsutrymme</span><span class="sxs-lookup"><span data-stu-id="cddcf-184">Create an HDInsight Linux cluster with Data Lake Store as additional storage</span></span>

<span data-ttu-id="cddcf-185">I det här avsnittet skapar vi ett HDInsight Hadoop Linux-kluster med Data Lake Store som ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="cddcf-185">In this section, we create an HDInsight Hadoop Linux cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="cddcf-186">Hej HDInsight-kluster och hello Data Lake Store för den här versionen måste vara i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="cddcf-186">For this release, hello HDInsight cluster and hello Data Lake Store must be in hello same location.</span></span>

1. <span data-ttu-id="cddcf-187">Börja med att hämta hello prenumeration klient-ID.</span><span class="sxs-lookup"><span data-stu-id="cddcf-187">Start with retrieving hello subscription tenant ID.</span></span> <span data-ttu-id="cddcf-188">Du behöver som senare.</span><span class="sxs-lookup"><span data-stu-id="cddcf-188">You will need that later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. <span data-ttu-id="cddcf-189">Den här versionen kan för Hadoop-kluster Data Lake Store endast användas som ett ytterligare lagringsutrymme för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cddcf-189">For this release, for a Hadoop cluster, Data Lake Store can only be used as an additional storage for hello cluster.</span></span> <span data-ttu-id="cddcf-190">hello standardlagring kommer fortfarande att hello Azure storage-blobbar (WASB).</span><span class="sxs-lookup"><span data-stu-id="cddcf-190">hello default storage will still be hello Azure storage blobs (WASB).</span></span> <span data-ttu-id="cddcf-191">Så måste vi först skapa hello storage-konto och lagringsbehållare som krävs för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cddcf-191">So, we'll first create hello storage account and storage containers required for hello cluster.</span></span>

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. <span data-ttu-id="cddcf-192">Skapa hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cddcf-192">Create hello HDInsight cluster.</span></span> <span data-ttu-id="cddcf-193">Använd hello följande cmdlet: ar.</span><span class="sxs-lookup"><span data-stu-id="cddcf-193">Use hello following cmdlets.</span></span>

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have hello same name for hello cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    <span data-ttu-id="cddcf-194">Du bör se utdata visar en lista över hello klusterinformation efter hello cmdleten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="cddcf-194">After hello cmdlet successfully completes, you should see an output listing hello cluster details.</span></span>


## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a><span data-ttu-id="cddcf-195">Kör testjobb på hello HDInsight-kluster toouse hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-195">Run test jobs on hello HDInsight cluster toouse hello Data Lake Store</span></span>
<span data-ttu-id="cddcf-196">När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på hello klustret tootest som hello HDInsight klustret kan komma åt Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-196">After you have configured an HDInsight cluster, you can run test jobs on hello cluster tootest that hello HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="cddcf-197">toodo så vi ska köra en Hive-jobb som skapar en tabell med hello exempeldata som du överfört tidigare tooyour Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-197">toodo so, we will run a sample Hive job that creates a table using hello sample data that you uploaded earlier tooyour Data Lake Store.</span></span>

<span data-ttu-id="cddcf-198">I det här avsnittet kommer du att SSH till hello HDInsight Linux klustret du skapade och kör hello en exempelfråga Hive.</span><span class="sxs-lookup"><span data-stu-id="cddcf-198">In this section you will SSH into hello HDInsight Linux cluster you created and run hello a sample Hive query.</span></span>

* <span data-ttu-id="cddcf-199">Om du använder en Windows-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="cddcf-199">If you are using a Windows client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="cddcf-200">Om du använder en Linux-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="cddcf-200">If you are using a Linux client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

1. <span data-ttu-id="cddcf-201">När du är ansluten, starta hello Hive CLI med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cddcf-201">Once connected, start hello Hive CLI by using hello following command:</span></span>

        hive
2. <span data-ttu-id="cddcf-202">Med hjälp av hello CLI, ange hello följande instruktioner toocreate en ny tabell med namnet **fordon** med hello exempeldata i hello Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="cddcf-202">Using hello CLI, enter hello following statements toocreate a new table named **vehicles** by using hello sample data in hello Data Lake Store:</span></span>

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    <span data-ttu-id="cddcf-203">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="cddcf-203">You should see an output similar toohello following:</span></span>

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

## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="cddcf-204">Åtkomst till Data Lake Store med hjälp av HDFS-kommandon</span><span class="sxs-lookup"><span data-stu-id="cddcf-204">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="cddcf-205">När du har konfigurerat hello HDInsight klustret toouse Data Lake Store kan använda du hello HDFS shell-kommandon tooaccess hello store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-205">Once you have configured hello HDInsight cluster toouse Data Lake Store, you can use hello HDFS shell commands tooaccess hello store.</span></span>

<span data-ttu-id="cddcf-206">I det här avsnittet kommer du att SSH till hello HDInsight Linux klustret du skapade och kör hello HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="cddcf-206">In this section you will SSH into hello HDInsight Linux cluster you created and run hello HDFS commands.</span></span>

* <span data-ttu-id="cddcf-207">Om du använder en Windows-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="cddcf-207">If you are using a Windows client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="cddcf-208">Om du använder en Linux-klient tooSSH i hello kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="cddcf-208">If you are using a Linux client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

<span data-ttu-id="cddcf-209">När du är ansluten, Använd hello följande HDFS filesystem kommandot toolist hello filer i hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-209">Once connected, use hello following HDFS filesystem command toolist hello files in hello Data Lake Store.</span></span>

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

<span data-ttu-id="cddcf-210">Du bör nu se hello-fil som du överfört tidigare toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cddcf-210">This should list hello file that you uploaded earlier toohello Data Lake Store.</span></span>

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

<span data-ttu-id="cddcf-211">Du kan också använda hello `hdfs dfs -put` kommandot tooupload vissa filer toohello Data Lake Store och sedan använda `hdfs dfs -ls` tooverify hello filer har laddats.</span><span class="sxs-lookup"><span data-stu-id="cddcf-211">You can also use hello `hdfs dfs -put` command tooupload some files toohello Data Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="cddcf-212">Se även</span><span class="sxs-lookup"><span data-stu-id="cddcf-212">See Also</span></span>
* [<span data-ttu-id="cddcf-213">Portalen: Skapa ett HDInsight-kluster toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="cddcf-213">Portal: Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
