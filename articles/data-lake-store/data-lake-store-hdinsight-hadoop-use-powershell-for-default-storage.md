---
title: "Skapa HDInsight-kluster med Data Lake Store som standardlagring med hjälp av PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell för att skapa och använda HDInsight-kluster med Azure Data Lake Store"
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
ms.openlocfilehash: 77eb83b80312eca401e6f60d57ed6a5668ea442e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a><span data-ttu-id="464d7-103">Skapa HDInsight-kluster med Data Lake Store som standardlagring med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="464d7-103">Create HDInsight clusters with Data Lake Store as default storage by using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="464d7-104">Använda Azure Portal</span><span class="sxs-lookup"><span data-stu-id="464d7-104">Use the Azure portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="464d7-105">Använd PowerShell (för standardlagring)</span><span class="sxs-lookup"><span data-stu-id="464d7-105">Use PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="464d7-106">Använd PowerShell (för ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="464d7-106">Use PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="464d7-107">Använd Resource Manager</span><span class="sxs-lookup"><span data-stu-id="464d7-107">Use Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

<span data-ttu-id="464d7-108">Lär dig hur du konfigurerar Azure HDInsight-kluster med Azure Data Lake Store, som standardlagring med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="464d7-108">Learn how to use Azure PowerShell to configure Azure HDInsight clusters with Azure Data Lake Store, as default storage.</span></span> <span data-ttu-id="464d7-109">Anvisningar om hur du skapar ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme finns [skapar ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme](data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="464d7-109">For instructions on creating an HDInsight cluster with Data Lake Store as additional storage, see [Create an HDInsight cluster with Data Lake Store as additional storage](data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

<span data-ttu-id="464d7-110">Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="464d7-110">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="464d7-111">Alternativet för att skapa HDInsight-kluster med åtkomst till Data Lake Store som standardlagring är tillgänglig för HDInsight version 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="464d7-111">The option to create HDInsight clusters with access to Data Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="464d7-112">Alternativet för att skapa HDInsight-kluster med åtkomst till Data Lake Store eftersom standardlagring *inte tillgänglig* för HDInsight Premium-kluster.</span><span class="sxs-lookup"><span data-stu-id="464d7-112">The option to create HDInsight clusters with access to Data Lake Store as default storage is *not available* for HDInsight Premium clusters.</span></span>

<span data-ttu-id="464d7-113">Följ instruktionerna i följande fem avsnitt om du vill konfigurera HDInsight för att arbeta med Data Lake Store med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="464d7-113">To configure HDInsight to work with Data Lake Store by using PowerShell, follow the instructions in the next five sections.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="464d7-114">Krav</span><span class="sxs-lookup"><span data-stu-id="464d7-114">Prerequisites</span></span>
<span data-ttu-id="464d7-115">Innan du påbörjar den här självstudien måste du kontrollera att du uppfyller följande krav:</span><span class="sxs-lookup"><span data-stu-id="464d7-115">Before you begin this tutorial, make sure that you meet the following requirements:</span></span>

* <span data-ttu-id="464d7-116">**En Azure-prenumeration**: Gå till [hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="464d7-116">**An Azure subscription**: Go to [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="464d7-117">**Azure PowerShell 1.0 eller senare**: se [hur du installerar och konfigurerar PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="464d7-117">**Azure PowerShell 1.0 or greater**: See [How to install and configure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="464d7-118">**Windows Software Development Kit (SDK)**: Om du vill installera Windows SDK, gå till [laddar ned och verktyg för Windows 10](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="464d7-118">**Windows Software Development Kit (SDK)**: To install Windows SDK, go to [Downloads and tools for Windows 10](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="464d7-119">SDK: N används för att skapa ett säkerhetscertifikat.</span><span class="sxs-lookup"><span data-stu-id="464d7-119">The SDK is used to create a security certificate.</span></span>
* <span data-ttu-id="464d7-120">**Azure Active Directory-tjänstens huvudnamn**: den här självstudiekursen beskrivs hur du skapar ett huvudnamn för tjänsten i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="464d7-120">**Azure Active Directory service principal**: This tutorial describes how to create a service principal in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="464d7-121">Du måste dock vara en Azure AD-administratör om du vill skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="464d7-121">However, to create a service principal, you must be an Azure AD administrator.</span></span> <span data-ttu-id="464d7-122">Om du är administratör kan du gå vidare med självstudiekursen hoppa över det här kravet.</span><span class="sxs-lookup"><span data-stu-id="464d7-122">If you are an administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    >[!NOTE]
    ><span data-ttu-id="464d7-123">Du kan skapa en tjänst huvudnamn endast om du är administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="464d7-123">You can create a service principal only if you are an Azure AD administrator.</span></span> <span data-ttu-id="464d7-124">Azure AD-administratör skapa en tjänst huvudnamn innan du kan skapa ett HDInsight-kluster med Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="464d7-124">Your Azure AD administrator must create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="464d7-125">Tjänstens huvudnamn måste skapas med ett certifikat som beskrivs i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="464d7-125">The service principal must be created with a certificate, as described in [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>
    >

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="464d7-126">Skapa ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="464d7-126">Create a Data Lake Store account</span></span>
<span data-ttu-id="464d7-127">Om du vill skapa ett Data Lake Store-konto, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="464d7-127">To create a Data Lake Store account, do the following:</span></span>

1. <span data-ttu-id="464d7-128">Öppna ett PowerShell-fönster på skrivbordet och ange sedan fragmenten nedan.</span><span class="sxs-lookup"><span data-stu-id="464d7-128">From your desktop, open a PowerShell window, and then enter the snippets below.</span></span> <span data-ttu-id="464d7-129">När du uppmanas att logga in, logga in som en prenumerationsadministratörer eller ägare.</span><span class="sxs-lookup"><span data-stu-id="464d7-129">When you are prompted to sign in, sign in as one of the subscription administrators or owners.</span></span> 

        # Sign in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > <span data-ttu-id="464d7-130">Om du registerresursleverantören Data Lake Store och får ett felmeddelande liknande `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, prenumerationen kanske inte är godkända för Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="464d7-130">If you register the Data Lake Store resource provider and receive an error similar to `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, your subscription might not be whitelisted for Data Lake Store.</span></span> <span data-ttu-id="464d7-131">Om du vill aktivera din Azure-prenumeration för Data Lake Store förhandsversion följer du anvisningarna i [Kom igång med Azure Data Lake Store med hjälp av Azure portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="464d7-131">To enable your Azure subscription for the Data Lake Store public preview, follow the instructions in [Get started with Azure Data Lake Store by using the Azure portal](data-lake-store-get-started-portal.md).</span></span>
    >

2. <span data-ttu-id="464d7-132">Ett Data Lake Store-konto är kopplat till en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="464d7-132">A Data Lake Store account is associated with an Azure resource group.</span></span> <span data-ttu-id="464d7-133">Börja med att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="464d7-133">Start by creating a resource group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="464d7-134">Du bör se utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="464d7-134">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="464d7-135">Skapa ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="464d7-135">Create a Data Lake Store account.</span></span> <span data-ttu-id="464d7-136">Namnet på kontot du anger måste innehålla endast små bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="464d7-136">The account name you specify must contain only lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="464d7-137">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="464d7-137">You should see an output like the following:</span></span>

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

4. <span data-ttu-id="464d7-138">Med hjälp av Data Lake Store som standardlagring måste du ange en rotsökväg som kopieras filerna klusterspecifika när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="464d7-138">Using Data Lake Store as default storage requires you to specify a root path to which the cluster-specific files are copied during cluster creation.</span></span> <span data-ttu-id="464d7-139">Så här skapar du en rotsökväg, vilket är **/kluster/hdiadlcluster** i fragment, använder du följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="464d7-139">To create a root path, which is **/clusters/hdiadlcluster** in the  snippet, use the following cmdlets:</span></span>

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a><span data-ttu-id="464d7-140">Konfigurera autentisering för rollbaserad åtkomst till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="464d7-140">Set up authentication for role-based access to Data Lake Store</span></span>
<span data-ttu-id="464d7-141">Alla Azure-prenumerationer är associerade med en Azure AD-entitet.</span><span class="sxs-lookup"><span data-stu-id="464d7-141">Every Azure subscription is associated with an Azure AD entity.</span></span> <span data-ttu-id="464d7-142">Användare och tjänster som har åtkomst till prenumerationsresurser med hjälp av Azure-portalen eller Azure Resource Manager API måste först autentisera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="464d7-142">Users and services that access subscription resources by using the Azure portal or the Azure Resource Manager API must first authenticate with Azure AD.</span></span> <span data-ttu-id="464d7-143">Åtkomst till Azure-prenumerationer och tjänster genom att tilldela dem sedan rätt roll för en Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="464d7-143">Access is granted to Azure subscriptions and services by assigning them the appropriate role on an Azure resource.</span></span> <span data-ttu-id="464d7-144">För tjänster identifierar en tjänst som tjänsten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="464d7-144">For services, a service principal identifies the service in Azure AD.</span></span>

<span data-ttu-id="464d7-145">Det här avsnittet beskriver hur du ger en tjänst för programmet, till exempel HDInsight, åtkomst till en Azure-resurs (Data Lake Store-kontot som du skapade tidigare).</span><span class="sxs-lookup"><span data-stu-id="464d7-145">This section illustrates how to grant an application service, such as HDInsight, access to an Azure resource (the Data Lake Store account that you created earlier).</span></span> <span data-ttu-id="464d7-146">Det gör du genom att skapa en tjänst huvudnamn för programmet och tilldela roller till den via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="464d7-146">You do so by creating a service principal for the application and assigning roles to it via PowerShell.</span></span>

<span data-ttu-id="464d7-147">Om du vill konfigurera Active Directory-autentisering för Azure Data Lake utför du uppgifterna i följande två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="464d7-147">To set up Active Directory authentication for Azure Data Lake, perform the tasks in the following two sections.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="464d7-148">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="464d7-148">Create a self-signed certificate</span></span>
<span data-ttu-id="464d7-149">Kontrollera att du har [Windows SDK](https://dev.windows.com/en-us/downloads) installerad innan du fortsätter med instruktionerna i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="464d7-149">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with the steps in this section.</span></span> <span data-ttu-id="464d7-150">Måste du också skapa en katalog som *C:\mycertdir*, där du kan skapa certifikatet.</span><span class="sxs-lookup"><span data-stu-id="464d7-150">You must have also created a directory, such as *C:\mycertdir*, where you create the certificate.</span></span>

1. <span data-ttu-id="464d7-151">Gå till den plats där du har installerat Windows SDK från PowerShell-fönster (vanligtvis *C:\Program Files (x86) \Windows Kits\10\bin\x86*) och använda den [MakeCert] [ makecert] för att skapa ett självsignerat certifikat och en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="464d7-151">From the PowerShell window, go to the location where you installed Windows SDK (typically, *C:\Program Files (x86)\Windows Kits\10\bin\x86*) and use the [MakeCert][makecert] utility to create a self-signed certificate and a private key.</span></span> <span data-ttu-id="464d7-152">Använd följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="464d7-152">Use the following commands:</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="464d7-153">Du uppmanas att ange lösenordet för privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="464d7-153">You will be prompted to enter the private key password.</span></span> <span data-ttu-id="464d7-154">Efter att kommandot har körts, bör du se **CertFile.cer** och **mykey.pvk** i katalogen certifikat som du angav.</span><span class="sxs-lookup"><span data-stu-id="464d7-154">After the command is successfully executed, you should see **CertFile.cer** and **mykey.pvk** in the certificate directory that you specified.</span></span>
2. <span data-ttu-id="464d7-155">Använd den [Pvk2Pfx] [ pvk2pfx] verktyg för att konvertera .pvk och .cer-filer som skapats av MakeCert till en .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="464d7-155">Use the [Pvk2Pfx][pvk2pfx] utility to convert the .pvk and .cer files that MakeCert created to a .pfx file.</span></span> <span data-ttu-id="464d7-156">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="464d7-156">Run the following command:</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="464d7-157">När du uppmanas ange privata nyckel som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="464d7-157">When you are prompted, enter the private key password that you specified earlier.</span></span> <span data-ttu-id="464d7-158">Värdet som du anger för den **IO -** parameter är det lösenord som är associerad med den .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="464d7-158">The value you specify for the **-po** parameter is the password that's associated with the .pfx file.</span></span> <span data-ttu-id="464d7-159">När kommandot har slutförts, bör du också se en **CertFile.pfx** i katalogen certifikat som du angav.</span><span class="sxs-lookup"><span data-stu-id="464d7-159">After the command has been completed successfully, you should also see a **CertFile.pfx** in the certificate directory that you specified.</span></span>

### <a name="create-an-azure-ad-and-a-service-principal"></a><span data-ttu-id="464d7-160">Skapa en Azure AD och ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="464d7-160">Create an Azure AD and a service principal</span></span>
<span data-ttu-id="464d7-161">I det här avsnittet, skapa ett huvudnamn för tjänsten för Azure AD-program, tilldela en roll till tjänstens huvudnamn och autentisera sig som tjänstens huvudnamn genom att tillhandahålla ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="464d7-161">In this section, you create a service principal for an Azure AD application, assign a role to the service principal, and authenticate as the service principal by providing a certificate.</span></span> <span data-ttu-id="464d7-162">Om du vill skapa ett program i Azure AD, kör du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="464d7-162">To create an application in Azure AD, run the following commands:</span></span>

1. <span data-ttu-id="464d7-163">Klistra in följande cmdlets i PowerShell-konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="464d7-163">Paste the following cmdlets in the PowerShell console window.</span></span> <span data-ttu-id="464d7-164">Se till att värdet som du anger för den **- DisplayName** egenskapen är unika.</span><span class="sxs-lookup"><span data-stu-id="464d7-164">Make sure that the value you specify for the **-DisplayName** property is unique.</span></span> <span data-ttu-id="464d7-165">Värdena för **- webbsida** och **- IdentiferUris** är platshållarvärdena och kan inte verifieras.</span><span class="sxs-lookup"><span data-stu-id="464d7-165">The values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

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
2. <span data-ttu-id="464d7-166">Skapa ett huvudnamn för tjänsten med hjälp av program-ID.</span><span class="sxs-lookup"><span data-stu-id="464d7-166">Create a service principal by using the application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="464d7-167">Ge service principal åtkomst till Data Lake Store-roten och alla mappar i rotsökvägen som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="464d7-167">Grant the service principal access to the Data Lake Store root and all the folders in the root path that you specified earlier.</span></span> <span data-ttu-id="464d7-168">Använd följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="464d7-168">Use the following cmdlets:</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a><span data-ttu-id="464d7-169">Skapa ett kluster i HDInsight Linux med Data Lake Store som standardlagring</span><span class="sxs-lookup"><span data-stu-id="464d7-169">Create an HDInsight Linux cluster with Data Lake Store as the default storage</span></span>

<span data-ttu-id="464d7-170">I det här avsnittet skapar du ett HDInsight Hadoop Linux-kluster med Data Lake Store som standardlagring.</span><span class="sxs-lookup"><span data-stu-id="464d7-170">In this section, you create an HDInsight Hadoop Linux cluster with Data Lake Store as the default storage.</span></span> <span data-ttu-id="464d7-171">HDInsight-kluster och Data Lake Store måste vara på samma plats för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="464d7-171">For this release, the HDInsight cluster and Data Lake Store must be in the same location.</span></span>

1. <span data-ttu-id="464d7-172">Hämta klient-ID för prenumerationen och spara den för senare användning.</span><span class="sxs-lookup"><span data-stu-id="464d7-172">Retrieve the subscription tenant ID, and store it to use later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. <span data-ttu-id="464d7-173">Skapa HDInsight-kluster med hjälp av följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="464d7-173">Create the HDInsight cluster by using the following cmdlets:</span></span>

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
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

    <span data-ttu-id="464d7-174">Du bör se utdata som visar information om kluster när cmdleten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="464d7-174">After the cmdlet has been successfully completed, you should see an output that lists the cluster details.</span></span>

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a><span data-ttu-id="464d7-175">Kör testjobb i HDInsight-klustret för att använda Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="464d7-175">Run test jobs on the HDInsight cluster to use Data Lake Store</span></span>
<span data-ttu-id="464d7-176">När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på den så att den kan komma åt Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="464d7-176">After you have configured an HDInsight cluster, you can run test jobs on it to ensure that it can access Data Lake Store.</span></span> <span data-ttu-id="464d7-177">Gör du genom att köra ett exempel Hive-jobb för att skapa en tabell som använder exempeldata som redan finns i Data Lake Store på  *<cluster root>/example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="464d7-177">To do so, run a sample Hive job to create a table that uses the sample data that's already available in Data Lake Store at *<cluster root>/example/data/sample.log*.</span></span>

<span data-ttu-id="464d7-178">Du upprättar en anslutning för SSH (Secure Shell) i HDInsight Linux-kluster som du skapade i det här avsnittet och kör sedan en exempelfråga Hive.</span><span class="sxs-lookup"><span data-stu-id="464d7-178">In this section, you make a Secure Shell (SSH) connection into the HDInsight Linux cluster that you created, and then you run a sample Hive query.</span></span>

* <span data-ttu-id="464d7-179">Om du använder en Windows-klient för att göra en SSH-anslutning till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="464d7-179">If you are using a Windows client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="464d7-180">Om du använder en Linux-klient för att göra en SSH-anslutning till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="464d7-180">If you are using a Linux client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="464d7-181">När du har gjort anslutningen, startar du Hive-kommandoradsgränssnittet (CLI) med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="464d7-181">After you have made the connection, start the Hive command-line interface (CLI) by using the following command:</span></span>

        hive
2. <span data-ttu-id="464d7-182">Använd CLI för att ange följande instruktioner om du vill skapa en ny tabell med namnet **fordon** med exempeldata i Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="464d7-182">Use the CLI to enter the following statements to create a new table named **vehicles** by using the sample data in Data Lake Store:</span></span>

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="464d7-183">Du bör se utdata på konsolen SSH.</span><span class="sxs-lookup"><span data-stu-id="464d7-183">You should see the query output on the SSH console.</span></span>

    >[!NOTE]
    ><span data-ttu-id="464d7-184">Sökvägen till exempeldata i föregående CREATE TABLE-kommando är `adl:///example/data/`, där `adl:///` är roten till klustret.</span><span class="sxs-lookup"><span data-stu-id="464d7-184">The path to the sample data in the preceding CREATE TABLE command is `adl:///example/data/`, where `adl:///` is the cluster root.</span></span> <span data-ttu-id="464d7-185">Följande exempel för rot-kluster som har angetts i den här självstudiekursen kommandot är `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span><span class="sxs-lookup"><span data-stu-id="464d7-185">Following the example of the cluster root that's specified in this tutorial, the command is `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span></span> <span data-ttu-id="464d7-186">Du kan använda kortare alternativ, eller så kan du ange den fullständiga sökvägen till kluster-roten.</span><span class="sxs-lookup"><span data-stu-id="464d7-186">You can either use the shorter alternative or provide the complete path to the cluster root.</span></span>
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a><span data-ttu-id="464d7-187">Åtkomst till Data Lake Store med hjälp av HDFS-kommandon</span><span class="sxs-lookup"><span data-stu-id="464d7-187">Access Data Lake Store by using HDFS commands</span></span>
<span data-ttu-id="464d7-188">När du har konfigurerat HDInsight-klustret för att använda Data Lake Store kan använda du Hadoop Distributed File System (HDFS)-gränssnittskommandon tillgång till store.</span><span class="sxs-lookup"><span data-stu-id="464d7-188">After you have configured the HDInsight cluster to use Data Lake Store, you can use Hadoop Distributed File System (HDFS) shell commands to access the store.</span></span>

<span data-ttu-id="464d7-189">Du gör en SSH-anslutning i HDInsight Linux-kluster som du skapade i det här avsnittet och kör sedan HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="464d7-189">In this section, you make an SSH connection into the HDInsight Linux cluster that you created, and then you run the HDFS commands.</span></span>

* <span data-ttu-id="464d7-190">Om du använder en Windows-klient för att göra en SSH-anslutning till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="464d7-190">If you are using a Windows client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="464d7-191">Om du använder en Linux-klient för att göra en SSH-anslutning till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="464d7-191">If you are using a Linux client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="464d7-192">När du har gjort anslutningen kan du visa filer i Data Lake Store med hjälp av följande kommando för HDFS file system.</span><span class="sxs-lookup"><span data-stu-id="464d7-192">After you've made the connection, list the files in Data Lake Store by using the following HDFS file system command.</span></span>

    hdfs dfs -ls adl:///

<span data-ttu-id="464d7-193">Du kan också använda den `hdfs dfs -put` kommando för att överföra filer till Data Lake Store och sedan använda `hdfs dfs -ls` att kontrollera om filerna som har laddats upp.</span><span class="sxs-lookup"><span data-stu-id="464d7-193">You can also use the `hdfs dfs -put` command to upload some files to Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="464d7-194">Se även</span><span class="sxs-lookup"><span data-stu-id="464d7-194">See also</span></span>
* [<span data-ttu-id="464d7-195">Azure-portalen: skapa ett HDInsight-kluster om du vill använda Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="464d7-195">Azure portal: Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
