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
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a><span data-ttu-id="6fcc7-103">Skapa HDInsight-kluster med Data Lake Store som standardlagring med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6fcc7-103">Create HDInsight clusters with Data Lake Store as default storage by using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6fcc7-104">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6fcc7-104">Use hello Azure portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="6fcc7-105">Använd PowerShell (för standardlagring)</span><span class="sxs-lookup"><span data-stu-id="6fcc7-105">Use PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="6fcc7-106">Använd PowerShell (för ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="6fcc7-106">Use PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="6fcc7-107">Använd Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6fcc7-107">Use Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

<span data-ttu-id="6fcc7-108">Lär dig hur toouse Azure PowerShell tooconfigure Azure HDInsight-kluster med Azure Data Lake Store, som standardlagring.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-108">Learn how toouse Azure PowerShell tooconfigure Azure HDInsight clusters with Azure Data Lake Store, as default storage.</span></span> <span data-ttu-id="6fcc7-109">Anvisningar om hur du skapar ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme finns [skapar ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme](data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-109">For instructions on creating an HDInsight cluster with Data Lake Store as additional storage, see [Create an HDInsight cluster with Data Lake Store as additional storage](data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

<span data-ttu-id="6fcc7-110">Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-110">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="6fcc7-111">hello alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som standardlagring är tillgänglig för HDInsight version 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-111">hello option toocreate HDInsight clusters with access tooData Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="6fcc7-112">hello alternativet toocreate HDInsight-kluster med åtkomst till tooData Datasjölager eftersom standardlagring *inte tillgänglig* för HDInsight Premium-kluster.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-112">hello option toocreate HDInsight clusters with access tooData Lake Store as default storage is *not available* for HDInsight Premium clusters.</span></span>

<span data-ttu-id="6fcc7-113">tooconfigure HDInsight toowork med Data Lake Store med hjälp av PowerShell, följ instruktionerna för hello i följande fem hello-avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-113">tooconfigure HDInsight toowork with Data Lake Store by using PowerShell, follow hello instructions in hello next five sections.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fcc7-114">Krav</span><span class="sxs-lookup"><span data-stu-id="6fcc7-114">Prerequisites</span></span>
<span data-ttu-id="6fcc7-115">Innan du påbörjar den här självstudien måste du kontrollera att du uppfyller följande krav hello:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-115">Before you begin this tutorial, make sure that you meet hello following requirements:</span></span>

* <span data-ttu-id="6fcc7-116">**En Azure-prenumeration**: gå för[hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-116">**An Azure subscription**: Go too[Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="6fcc7-117">**Azure PowerShell 1.0 eller senare**: se [hur tooinstall och konfigurerar PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-117">**Azure PowerShell 1.0 or greater**: See [How tooinstall and configure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="6fcc7-118">**Windows Software Development Kit (SDK)**: tooinstall Windows SDK och gå för[laddar ned och verktyg för Windows 10](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-118">**Windows Software Development Kit (SDK)**: tooinstall Windows SDK, go too[Downloads and tools for Windows 10](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="6fcc7-119">hello SDK är används toocreate säkerhetscertifikat.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-119">hello SDK is used toocreate a security certificate.</span></span>
* <span data-ttu-id="6fcc7-120">**Azure Active Directory-tjänstens huvudnamn**: den här självstudiekursen beskriver hur toocreate ett huvudnamn för tjänsten i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-120">**Azure Active Directory service principal**: This tutorial describes how toocreate a service principal in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="6fcc7-121">Dock toocreate ett huvudnamn för tjänsten, måste du vara administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-121">However, toocreate a service principal, you must be an Azure AD administrator.</span></span> <span data-ttu-id="6fcc7-122">Om du är administratör kan du hoppa över det här kravet och fortsätt med hello kursen.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-122">If you are an administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    >[!NOTE]
    ><span data-ttu-id="6fcc7-123">Du kan skapa en tjänst huvudnamn endast om du är administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-123">You can create a service principal only if you are an Azure AD administrator.</span></span> <span data-ttu-id="6fcc7-124">Azure AD-administratör skapa en tjänst huvudnamn innan du kan skapa ett HDInsight-kluster med Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-124">Your Azure AD administrator must create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="6fcc7-125">hello tjänstens huvudnamn måste skapas med ett certifikat som beskrivs i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-125">hello service principal must be created with a certificate, as described in [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>
    >

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="6fcc7-126">Skapa ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="6fcc7-126">Create a Data Lake Store account</span></span>
<span data-ttu-id="6fcc7-127">toocreate ett Data Lake Store-konto hello följande:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-127">toocreate a Data Lake Store account, do hello following:</span></span>

1. <span data-ttu-id="6fcc7-128">Öppna ett PowerShell-fönster på skrivbordet och ange sedan hello fragmenten nedan.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-128">From your desktop, open a PowerShell window, and then enter hello snippets below.</span></span> <span data-ttu-id="6fcc7-129">När du kan ange toosign i logga in som en hello prenumerationsadministratörer eller ägare.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-129">When you are prompted toosign in, sign in as one of hello subscription administrators or owners.</span></span> 

        # Sign in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > <span data-ttu-id="6fcc7-130">Om du registerresursleverantören hello Data Lake Store och ett felmeddelande liknande för`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, prenumerationen kanske inte är godkända för Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-130">If you register hello Data Lake Store resource provider and receive an error similar too`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, your subscription might not be whitelisted for Data Lake Store.</span></span> <span data-ttu-id="6fcc7-131">tooenable din Azure-prenumeration för hello Data Lake Store public preview, följer du anvisningarna hello i [Kom igång med Azure Data Lake Store med hjälp av hello Azure-portalen](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-131">tooenable your Azure subscription for hello Data Lake Store public preview, follow hello instructions in [Get started with Azure Data Lake Store by using hello Azure portal](data-lake-store-get-started-portal.md).</span></span>
    >

2. <span data-ttu-id="6fcc7-132">Ett Data Lake Store-konto är kopplat till en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-132">A Data Lake Store account is associated with an Azure resource group.</span></span> <span data-ttu-id="6fcc7-133">Börja med att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-133">Start by creating a resource group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="6fcc7-134">Du bör se utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-134">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="6fcc7-135">Skapa ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-135">Create a Data Lake Store account.</span></span> <span data-ttu-id="6fcc7-136">hello konto namn som du anger måste innehålla endast små bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-136">hello account name you specify must contain only lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="6fcc7-137">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-137">You should see an output like hello following:</span></span>

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

4. <span data-ttu-id="6fcc7-138">Om du använder Data Lake Store som standardlagring måste toospecify rot sökvägen toowhich hello klusterspecifika filer kopieras när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-138">Using Data Lake Store as default storage requires you toospecify a root path toowhich hello cluster-specific files are copied during cluster creation.</span></span> <span data-ttu-id="6fcc7-139">toocreate en rotsökväg, vilket är **/kluster/hdiadlcluster** hello kodutdrag använda hello följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-139">toocreate a root path, which is **/clusters/hdiadlcluster** in hello  snippet, use hello following cmdlets:</span></span>

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a><span data-ttu-id="6fcc7-140">Konfigurera autentisering för rollbaserad åtkomst tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="6fcc7-140">Set up authentication for role-based access tooData Lake Store</span></span>
<span data-ttu-id="6fcc7-141">Alla Azure-prenumerationer är associerade med en Azure AD-entitet.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-141">Every Azure subscription is associated with an Azure AD entity.</span></span> <span data-ttu-id="6fcc7-142">Användare och tjänster för att komma åt prenumerationsresurser med hjälp av hello Azure-portalen eller hello Azure Resource Manager API måste först autentisera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-142">Users and services that access subscription resources by using hello Azure portal or hello Azure Resource Manager API must first authenticate with Azure AD.</span></span> <span data-ttu-id="6fcc7-143">Komma åt tooAzure prenumerationer och tjänster genom att tilldela dem till lämpliga hello-rollen på en Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-143">Access is granted tooAzure subscriptions and services by assigning them hello appropriate role on an Azure resource.</span></span> <span data-ttu-id="6fcc7-144">För tjänster identifierar ett huvudnamn för tjänsten hello tjänst i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-144">For services, a service principal identifies hello service in Azure AD.</span></span>

<span data-ttu-id="6fcc7-145">Detta avsnitt visar hur toogrant ett program service, till exempel HDInsight, åtkomst tooan Azure-resurs (hello Data Lake Store-konto som du skapade tidigare).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-145">This section illustrates how toogrant an application service, such as HDInsight, access tooan Azure resource (hello Data Lake Store account that you created earlier).</span></span> <span data-ttu-id="6fcc7-146">Det gör du genom att skapa en tjänst huvudnamn för programmet hello och tilldela roller tooit via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-146">You do so by creating a service principal for hello application and assigning roles tooit via PowerShell.</span></span>

<span data-ttu-id="6fcc7-147">tooset in Active Directory-autentisering för Azure Data Lake utför hello uppgifter i hello följande två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-147">tooset up Active Directory authentication for Azure Data Lake, perform hello tasks in hello following two sections.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="6fcc7-148">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="6fcc7-148">Create a self-signed certificate</span></span>
<span data-ttu-id="6fcc7-149">Kontrollera att du har [Windows SDK](https://dev.windows.com/en-us/downloads) installerad innan du fortsätter med hello stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-149">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with hello steps in this section.</span></span> <span data-ttu-id="6fcc7-150">Måste du också skapa en katalog som *C:\mycertdir*, där du skapar hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-150">You must have also created a directory, such as *C:\mycertdir*, where you create hello certificate.</span></span>

1. <span data-ttu-id="6fcc7-151">Gå toohello plats där du har installerat Windows SDK från hello PowerShell-fönster (vanligtvis *C:\Program Files (x86) \Windows Kits\10\bin\x86*) och använda hello [MakeCert] [ makecert] verktyget toocreate ett självsignerat certifikat och en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-151">From hello PowerShell window, go toohello location where you installed Windows SDK (typically, *C:\Program Files (x86)\Windows Kits\10\bin\x86*) and use hello [MakeCert][makecert] utility toocreate a self-signed certificate and a private key.</span></span> <span data-ttu-id="6fcc7-152">Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-152">Use hello following commands:</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="6fcc7-153">Du kommer att tillfrågas tooenter hello lösenordet privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-153">You will be prompted tooenter hello private key password.</span></span> <span data-ttu-id="6fcc7-154">Efter hello-kommandot har körts, bör du se **CertFile.cer** och **mykey.pvk** i hello certifikat katalog som du angav.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-154">After hello command is successfully executed, you should see **CertFile.cer** and **mykey.pvk** in hello certificate directory that you specified.</span></span>
2. <span data-ttu-id="6fcc7-155">Använd hello [Pvk2Pfx] [ pvk2pfx] verktyget tooconvert hello .pvk och .cer-filer som MakeCert skapade tooa .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-155">Use hello [Pvk2Pfx][pvk2pfx] utility tooconvert hello .pvk and .cer files that MakeCert created tooa .pfx file.</span></span> <span data-ttu-id="6fcc7-156">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-156">Run hello following command:</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="6fcc7-157">När du uppmanas ange hello privata nyckel lösenord som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-157">When you are prompted, enter hello private key password that you specified earlier.</span></span> <span data-ttu-id="6fcc7-158">Hej värdet som du anger för hello **IO -** parameter är hello lösenord som är associerad med hello .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-158">hello value you specify for hello **-po** parameter is hello password that's associated with hello .pfx file.</span></span> <span data-ttu-id="6fcc7-159">När hello-kommandot har slutförts, bör du också se en **CertFile.pfx** i hello certifikat katalog som du angav.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-159">After hello command has been completed successfully, you should also see a **CertFile.pfx** in hello certificate directory that you specified.</span></span>

### <a name="create-an-azure-ad-and-a-service-principal"></a><span data-ttu-id="6fcc7-160">Skapa en Azure AD och ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="6fcc7-160">Create an Azure AD and a service principal</span></span>
<span data-ttu-id="6fcc7-161">I det här avsnittet, skapa ett huvudnamn för tjänsten för Azure AD-program, tilldela en roll toohello tjänstens huvudnamn och autentisera sig som hello tjänstens huvudnamn genom att tillhandahålla ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-161">In this section, you create a service principal for an Azure AD application, assign a role toohello service principal, and authenticate as hello service principal by providing a certificate.</span></span> <span data-ttu-id="6fcc7-162">toocreate ett program i Azure AD, kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-162">toocreate an application in Azure AD, run hello following commands:</span></span>

1. <span data-ttu-id="6fcc7-163">Klistra in följande cmdletar i PowerShell-konsolfönster för hello hello.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-163">Paste hello following cmdlets in hello PowerShell console window.</span></span> <span data-ttu-id="6fcc7-164">Kontrollera att hello-värde som du anger för hello **- DisplayName** egenskapen är unika.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-164">Make sure that hello value you specify for hello **-DisplayName** property is unique.</span></span> <span data-ttu-id="6fcc7-165">Hej värden för **- webbsida** och **- IdentiferUris** är platshållarvärdena och kan inte verifieras.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-165">hello values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

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
2. <span data-ttu-id="6fcc7-166">Skapa ett huvudnamn för tjänsten med hjälp av hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-166">Create a service principal by using hello application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="6fcc7-167">Bevilja hello service principal åtkomst toohello Data Lake Store rot och alla hello mappar i hello rotsökvägen som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-167">Grant hello service principal access toohello Data Lake Store root and all hello folders in hello root path that you specified earlier.</span></span> <span data-ttu-id="6fcc7-168">Använd hello följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-168">Use hello following cmdlets:</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-hello-default-storage"></a><span data-ttu-id="6fcc7-169">Skapa ett kluster i HDInsight Linux med Data Lake Store som hello standardlagring</span><span class="sxs-lookup"><span data-stu-id="6fcc7-169">Create an HDInsight Linux cluster with Data Lake Store as hello default storage</span></span>

<span data-ttu-id="6fcc7-170">I det här avsnittet skapar du ett HDInsight Hadoop Linux-kluster med Data Lake Store som hello standardlagring.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-170">In this section, you create an HDInsight Hadoop Linux cluster with Data Lake Store as hello default storage.</span></span> <span data-ttu-id="6fcc7-171">Den här versionen hello HDInsight-kluster och Data Lake Store måste vara i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-171">For this release, hello HDInsight cluster and Data Lake Store must be in hello same location.</span></span>

1. <span data-ttu-id="6fcc7-172">Hämta hello prenumeration klient-ID och lagra den toouse senare.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-172">Retrieve hello subscription tenant ID, and store it toouse later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. <span data-ttu-id="6fcc7-173">Skapa hello HDInsight-kluster med hjälp av hello följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-173">Create hello HDInsight cluster by using hello following cmdlets:</span></span>

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

    <span data-ttu-id="6fcc7-174">Du bör se utdata som visar hello klusterinformation när hello cmdleten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-174">After hello cmdlet has been successfully completed, you should see an output that lists hello cluster details.</span></span>

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-data-lake-store"></a><span data-ttu-id="6fcc7-175">Kör testjobb på hello HDInsight-kluster toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6fcc7-175">Run test jobs on hello HDInsight cluster toouse Data Lake Store</span></span>
<span data-ttu-id="6fcc7-176">När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på den tooensure att få åtkomst till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-176">After you have configured an HDInsight cluster, you can run test jobs on it tooensure that it can access Data Lake Store.</span></span> <span data-ttu-id="6fcc7-177">toodo så kör ett exempel Hive-jobbet toocreate en tabell som använder hello exempeldata som redan finns i Data Lake Store på  *<cluster root>/example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-177">toodo so, run a sample Hive job toocreate a table that uses hello sample data that's already available in Data Lake Store at *<cluster root>/example/data/sample.log*.</span></span>

<span data-ttu-id="6fcc7-178">Du upprättar en anslutning för SSH (Secure Shell) till hello HDInsight Linux-kluster som du skapade i det här avsnittet och kör sedan en exempelfråga Hive.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-178">In this section, you make a Secure Shell (SSH) connection into hello HDInsight Linux cluster that you created, and then you run a sample Hive query.</span></span>

* <span data-ttu-id="6fcc7-179">Om du använder en Windows-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-179">If you are using a Windows client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="6fcc7-180">Om du använder en Linux-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-180">If you are using a Linux client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="6fcc7-181">När du har gjort hello anslutning startar du hello Hive-kommandoradsgränssnittet (CLI) med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-181">After you have made hello connection, start hello Hive command-line interface (CLI) by using hello following command:</span></span>

        hive
2. <span data-ttu-id="6fcc7-182">Använd hello CLI tooenter hello följande instruktioner toocreate en ny tabell med namnet **fordon** med hjälp av hello exempeldata i Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="6fcc7-182">Use hello CLI tooenter hello following statements toocreate a new table named **vehicles** by using hello sample data in Data Lake Store:</span></span>

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="6fcc7-183">Du bör se hello frågeresultatet på hello SSH-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-183">You should see hello query output on hello SSH console.</span></span>

    >[!NOTE]
    ><span data-ttu-id="6fcc7-184">hello sökvägen toohello exempeldata i hello föregående CREATE TABLE-kommando är `adl:///example/data/`, där `adl:///` är hello kluster roten.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-184">hello path toohello sample data in hello preceding CREATE TABLE command is `adl:///example/data/`, where `adl:///` is hello cluster root.</span></span> <span data-ttu-id="6fcc7-185">Följande hello kluster roten som anges i den här självstudiekursen hello exempelvis hello-kommandot är `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-185">Following hello example of hello cluster root that's specified in this tutorial, hello command is `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span></span> <span data-ttu-id="6fcc7-186">Du kan använda hello kortare alternativ, eller så kan du ange hello fullständig sökväg toohello kluster roten.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-186">You can either use hello shorter alternative or provide hello complete path toohello cluster root.</span></span>
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a><span data-ttu-id="6fcc7-187">Åtkomst till Data Lake Store med hjälp av HDFS-kommandon</span><span class="sxs-lookup"><span data-stu-id="6fcc7-187">Access Data Lake Store by using HDFS commands</span></span>
<span data-ttu-id="6fcc7-188">När du har konfigurerat hello HDInsight klustret toouse Data Lake Store kan använda du Hadoop Distributed File System (HDFS) shell-kommandon tooaccess hello store.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-188">After you have configured hello HDInsight cluster toouse Data Lake Store, you can use Hadoop Distributed File System (HDFS) shell commands tooaccess hello store.</span></span>

<span data-ttu-id="6fcc7-189">Du gör en SSH-anslutning till hello HDInsight Linux-kluster som du skapade i det här avsnittet och kör sedan hello HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-189">In this section, you make an SSH connection into hello HDInsight Linux cluster that you created, and then you run hello HDFS commands.</span></span>

* <span data-ttu-id="6fcc7-190">Om du använder en Windows-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-190">If you are using a Windows client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="6fcc7-191">Om du använder en Linux-klient toomake en SSH-anslutning till hello-kluster, se [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6fcc7-191">If you are using a Linux client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="6fcc7-192">När du har gjort hello anslutning fil listan hello filer i Data Lake Store med hjälp av följande HDFS hello Systemkommando.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-192">After you've made hello connection, list hello files in Data Lake Store by using hello following HDFS file system command.</span></span>

    hdfs dfs -ls adl:///

<span data-ttu-id="6fcc7-193">Du kan också använda hello `hdfs dfs -put` kommandot tooupload vissa filer tooData Lake Store och sedan använda `hdfs dfs -ls` tooverify hello filer har laddats.</span><span class="sxs-lookup"><span data-stu-id="6fcc7-193">You can also use hello `hdfs dfs -put` command tooupload some files tooData Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="6fcc7-194">Se även</span><span class="sxs-lookup"><span data-stu-id="6fcc7-194">See also</span></span>
* [<span data-ttu-id="6fcc7-195">Azure-portalen: skapa ett HDInsight-kluster toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6fcc7-195">Azure portal: Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
