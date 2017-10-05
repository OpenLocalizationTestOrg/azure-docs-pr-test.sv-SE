---
title: "PowerShell: Azure HDInsight-kluster med Data Lake Store som tilläggslagring | Microsoft Docs"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/08/2017
ms.author: nitinme
ms.openlocfilehash: 7a7069adab5742a9dae2833c13a1db57337a41a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="abe9c-102">Använda Azure PowerShell för att skapa ett HDInsight-kluster med Data Lake Store (som ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="abe9c-102">Use Azure PowerShell to create an HDInsight cluster with Data Lake Store (as additional storage)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="abe9c-103">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="abe9c-103">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="abe9c-104">Med hjälp av PowerShell (för standardlagring)</span><span class="sxs-lookup"><span data-stu-id="abe9c-104">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="abe9c-105">Med hjälp av PowerShell (för ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="abe9c-105">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="abe9c-106">Med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="abe9c-106">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="abe9c-107">Lär dig hur du konfigurerar ett HDInsight-kluster med Azure Data Lake Store med hjälp av Azure PowerShell **som ytterligare lagringsutrymme**.</span><span class="sxs-lookup"><span data-stu-id="abe9c-107">Learn how to use Azure PowerShell to configure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span> <span data-ttu-id="abe9c-108">Anvisningar för hur du skapar ett HDInsight-kluster med Azure Data Lake Store som standardlagring finns [skapar ett HDInsight-kluster med Data Lake Store som standardlagring](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span><span class="sxs-lookup"><span data-stu-id="abe9c-108">For instructions on how to create an HDInsight cluster with Azure Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store as default storage](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span></span>

> [!NOTE]
> <span data-ttu-id="abe9c-109">Om du ska använda Azure Data Lake Store som ytterligare lagringsutrymme för HDInsight-kluster rekommenderar vi starkt att du gör detta när du skapar klustret, enligt beskrivningen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="abe9c-109">If you are going to use Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create the cluster as described in this article.</span></span> <span data-ttu-id="abe9c-110">Att lägga till Azure Data Lake Store som ytterligare lagringsutrymme för ett befintligt HDInsight-kluster är en komplicerad process där det är lätt att göra fel.</span><span class="sxs-lookup"><span data-stu-id="abe9c-110">Adding Azure Data Lake Store as additional storage to an existing HDInsight cluster is a complicated process and prone to errors.</span></span>
>

<span data-ttu-id="abe9c-111">Data Lake Store kan användas som en standardlagring eller ytterligare storage-konto för stöds klustertyper.</span><span class="sxs-lookup"><span data-stu-id="abe9c-111">For supported cluster types, Data Lake Store can be used as a default storage or additional storage account.</span></span> <span data-ttu-id="abe9c-112">När du använder Data Lake Store som ytterligare lagringsutrymme standardkontot för lagring för kluster kommer fortfarande att Azure Storage BLOB (WASB) och kluster-relaterade-filer (till exempel loggar osv.) är fortfarande skrivs till standardlagring medan de data som du vill bearbeta kan lagras i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="abe9c-112">When Data Lake Store is used as additional storage, the default storage account for the clusters will still be Azure Storage Blobs (WASB) and the cluster-related files (such as logs, etc.) are still written to the default storage, while the data that you want to process can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="abe9c-113">Med Data Lake Store som ett ytterligare storage-konto inte påverkar prestanda eller ge möjlighet att läsa eller skriva till lagring från klustret.</span><span class="sxs-lookup"><span data-stu-id="abe9c-113">Using Data Lake Store as an additional storage account does not impact performance or the ability to read/write to the storage from the cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="abe9c-114">Med hjälp av Data Lake Store för HDInsight klusterlagring</span><span class="sxs-lookup"><span data-stu-id="abe9c-114">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="abe9c-115">Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="abe9c-115">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="abe9c-116">Alternativet för att skapa HDInsight-kluster med åtkomst till Data Lake Store som ytterligare lagringsutrymmet är tillgängligt för HDInsight version 3.2, 3.4, 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="abe9c-116">Option to create HDInsight clusters with access to Data Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="abe9c-117">Konfigurera HDInsight för att arbeta med Data Lake Store omfattar med hjälp av PowerShell följande steg:</span><span class="sxs-lookup"><span data-stu-id="abe9c-117">Configuring HDInsight to work with Data Lake Store using PowerShell involves the following steps:</span></span>

* <span data-ttu-id="abe9c-118">Skapa ett Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-118">Create an Azure Data Lake Store</span></span>
* <span data-ttu-id="abe9c-119">Konfigurera autentisering för rollbaserad åtkomst till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-119">Set up authentication for role-based access to Data Lake Store</span></span>
* <span data-ttu-id="abe9c-120">Skapa HDInsight-kluster med autentisering till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-120">Create HDInsight cluster with authentication to Data Lake Store</span></span>
* <span data-ttu-id="abe9c-121">Köra ett testjobb på klustret</span><span class="sxs-lookup"><span data-stu-id="abe9c-121">Run a test job on the cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abe9c-122">Krav</span><span class="sxs-lookup"><span data-stu-id="abe9c-122">Prerequisites</span></span>
<span data-ttu-id="abe9c-123">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="abe9c-123">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="abe9c-124">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="abe9c-124">**An Azure subscription**.</span></span> <span data-ttu-id="abe9c-125">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abe9c-125">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="abe9c-126">**Installera Azure PowerShell 1.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="abe9c-126">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="abe9c-127">Se [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="abe9c-127">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="abe9c-128">**Windows SDK**.</span><span class="sxs-lookup"><span data-stu-id="abe9c-128">**Windows SDK**.</span></span> <span data-ttu-id="abe9c-129">Du kan installera den från [här](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="abe9c-129">You can install it from [here](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="abe9c-130">Du kan använda den för att skapa ett säkerhetscertifikat.</span><span class="sxs-lookup"><span data-stu-id="abe9c-130">You use this to create a security certificate.</span></span>
* <span data-ttu-id="abe9c-131">**Azure Active Directory Service Principal**.</span><span class="sxs-lookup"><span data-stu-id="abe9c-131">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="abe9c-132">Stegen i den här kursen ger instruktioner för hur du skapar ett huvudnamn för tjänsten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="abe9c-132">Steps in this tutorial provide instructions on how to create a service principal in Azure AD.</span></span> <span data-ttu-id="abe9c-133">Du måste dock vara en Azure AD-administratör för att kunna skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="abe9c-133">However, you must be an Azure AD administrator to be able to create a service principal.</span></span> <span data-ttu-id="abe9c-134">Om du är administratör för Azure AD kan du hoppa över det här kravet och fortsätta med guiden.</span><span class="sxs-lookup"><span data-stu-id="abe9c-134">If you are an Azure AD administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    <span data-ttu-id="abe9c-135">**Om du inte är en Azure AD-administratör**, kommer du inte kunna utföra stegen som krävs för att skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="abe9c-135">**If you are not an Azure AD administrator**, you will not be able to perform the steps required to create a service principal.</span></span> <span data-ttu-id="abe9c-136">I sådana fall måste din Azure AD-administratör först skapa ett huvudnamn för tjänsten innan du kan skapa ett HDInsight-kluster med Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-136">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="abe9c-137">Dessutom tjänstens huvudnamn måste skapas med hjälp av ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="abe9c-137">Also, the service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="abe9c-138">Skapa ett Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-138">Create an Azure Data Lake Store</span></span>
<span data-ttu-id="abe9c-139">Följ dessa steg om du vill skapa ett Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-139">Follow these steps to create a Data Lake Store.</span></span>

1. <span data-ttu-id="abe9c-140">Öppna ett nytt Azure PowerShell-fönster på skrivbordet och ange följande fragment.</span><span class="sxs-lookup"><span data-stu-id="abe9c-140">From your desktop, open a new Azure PowerShell window, and enter the following snippet.</span></span> <span data-ttu-id="abe9c-141">När du uppmanas att logga in, se till att logga in som en administratör/prenumerationsägaren:</span><span class="sxs-lookup"><span data-stu-id="abe9c-141">When prompted to log in, make sure you log in as one of the subscription administrator/owner:</span></span>

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > <span data-ttu-id="abe9c-142">Om du får ett felmeddelande liknande `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` när du registrerar Data Lake Store-resursprovidern är det möjligt att din prenumeration inte är godkända för Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-142">If you receive an error similar to `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` when registering the Data Lake Store resource provider, it is possible that your subscription is not whitelisted for Azure Data Lake Store.</span></span> <span data-ttu-id="abe9c-143">Kontrollera att du aktiverar din Azure-prenumeration för Data Lake Store public preview genom att följa dessa [instruktioner](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="abe9c-143">Make sure you enable your Azure subscription for Data Lake Store public preview by following these [instructions](data-lake-store-get-started-portal.md).</span></span>
   >
   >
2. <span data-ttu-id="abe9c-144">Ett Azure Data Lake Store-konto är kopplat till en resursgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="abe9c-144">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="abe9c-145">Börja med att skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="abe9c-145">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="abe9c-146">Du bör se utdata som liknar detta:</span><span class="sxs-lookup"><span data-stu-id="abe9c-146">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="abe9c-147">Skapa ett Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="abe9c-147">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="abe9c-148">Namnet på kontot du anger får bara innehålla gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="abe9c-148">The account name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="abe9c-149">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="abe9c-149">You should see an output like the following:</span></span>

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

5. <span data-ttu-id="abe9c-150">Ladda upp exempeldata till Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="abe9c-150">Upload some sample data to Azure Data Lake.</span></span> <span data-ttu-id="abe9c-151">Vi använder det senare i den här artikeln för att verifiera att data är tillgängliga från ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="abe9c-151">We'll use this later in this article to verify that the data is accessible from an HDInsight cluster.</span></span> <span data-ttu-id="abe9c-152">Om du behöver exempeldata att ladda upp, kan du hämta mappen **Ambulansdata** från [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="abe9c-152">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a><span data-ttu-id="abe9c-153">Konfigurera autentisering för rollbaserad åtkomst till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-153">Set up authentication for role-based access to Data Lake Store</span></span>
<span data-ttu-id="abe9c-154">Varje Azure-prenumeration är associerad med ett Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="abe9c-154">Every Azure subscription is associated with an Azure Active Directory.</span></span> <span data-ttu-id="abe9c-155">Användare och tjänster som har åtkomst till resurserna i prenumerationen med den klassiska Azure-portalen eller Azure Resource Manager API måste först autentisera med den Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="abe9c-155">Users and services that access resources of the subscription using the Azure Classic Portal or Azure Resource Manager API must first authenticate with that Azure Active Directory.</span></span> <span data-ttu-id="abe9c-156">Åtkomst till Azure-prenumerationer och tjänster genom att tilldela dem sedan rätt roll för en Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="abe9c-156">Access is granted to Azure subscriptions and services by assigning them the appropriate role on an Azure resource.</span></span>  <span data-ttu-id="abe9c-157">För tjänster identifierar ett huvudnamn för tjänsten tjänsten i Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="abe9c-157">For services, a service principal identifies the service in the Azure Active Directory (AAD).</span></span> <span data-ttu-id="abe9c-158">Det här avsnittet beskriver hur du ger en programtjänst som HDInsight, åtkomst till en Azure-resurs (det Azure Data Lake Store-konto du skapade tidigare) genom att skapa ett huvudnamn för tjänsten för programmet och tilldela roller till som via Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="abe9c-158">This section illustrates how to grant an application service, like HDInsight, access to an Azure resource (the Azure Data Lake Store account you created earlier) by creating a service principal for the application and assigning roles to that via Azure PowerShell.</span></span>

<span data-ttu-id="abe9c-159">Om du vill konfigurera Active Directory-autentisering för Azure Data Lake måste du utföra följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="abe9c-159">To set up Active Directory authentication for Azure Data Lake, you must perform the following tasks.</span></span>

* <span data-ttu-id="abe9c-160">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="abe9c-160">Create a self-signed certificate</span></span>
* <span data-ttu-id="abe9c-161">Skapa ett program i Azure Active Directory och ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="abe9c-161">Create an application in Azure Active Directory and a Service Principal</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="abe9c-162">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="abe9c-162">Create a self-signed certificate</span></span>
<span data-ttu-id="abe9c-163">Kontrollera att du har [Windows SDK](https://dev.windows.com/en-us/downloads) installerad innan du fortsätter med instruktionerna i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="abe9c-163">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with the steps in this section.</span></span> <span data-ttu-id="abe9c-164">Måste du också skapa en katalog som **C:\mycertdir**, där certifikatet ska skapas.</span><span class="sxs-lookup"><span data-stu-id="abe9c-164">You must have also created a directory, such as **C:\mycertdir**, where the certificate will be created.</span></span>

1. <span data-ttu-id="abe9c-165">Navigera till den plats där du har installerat Windows SDK från PowerShell-fönster (normalt `C:\Program Files (x86)\Windows Kits\10\bin\x86` och använda den [MakeCert] [ makecert] verktyg för att skapa ett självsignerat certifikat och en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="abe9c-165">From the PowerShell window, navigate to the location where you installed Windows SDK (typically, `C:\Program Files (x86)\Windows Kits\10\bin\x86` and use the [MakeCert][makecert] utility to create a self-signed certificate and a private key.</span></span> <span data-ttu-id="abe9c-166">Använd följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="abe9c-166">Use the following commands.</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="abe9c-167">Du uppmanas att ange lösenordet för privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="abe9c-167">You will be prompted to enter the private key password.</span></span> <span data-ttu-id="abe9c-168">När kommandot har körs, bör du se en **CertFile.cer** och **mykey.pvk** i katalogen certifikat som du har angett.</span><span class="sxs-lookup"><span data-stu-id="abe9c-168">After the command successfully executes, you should see a **CertFile.cer** and **mykey.pvk** in the certificate directory you specified.</span></span>
2. <span data-ttu-id="abe9c-169">Använd den [Pvk2Pfx] [ pvk2pfx] verktyg för att konvertera .pvk och .cer-filer som skapats av MakeCert till en .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="abe9c-169">Use the [Pvk2Pfx][pvk2pfx] utility to convert the .pvk and .cer files that MakeCert created to a .pfx file.</span></span> <span data-ttu-id="abe9c-170">Kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="abe9c-170">Run the following command.</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="abe9c-171">När du uppmanas ange privata nyckel lösenordet du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="abe9c-171">When prompted enter the private key password you specified earlier.</span></span> <span data-ttu-id="abe9c-172">Värdet som du anger för den **IO -** parameter är det lösenord som är associerad med den .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="abe9c-172">The value you specify for the **-po** parameter is the password that is associated with the .pfx file.</span></span> <span data-ttu-id="abe9c-173">Du bör också se en CertFile.pfx i katalogen certifikat som du angav när kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="abe9c-173">After the command successfully completes, you should also see a CertFile.pfx in the certificate directory you specified.</span></span>

### <a name="create-an-azure-active-directory-and-a-service-principal"></a><span data-ttu-id="abe9c-174">Skapa ett Azure Active Directory och ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="abe9c-174">Create an Azure Active Directory and a service principal</span></span>
<span data-ttu-id="abe9c-175">I det här avsnittet kan du utföra stegen för att skapa en tjänstens huvudnamn för ett Azure Active Directory-program, tilldela en roll till tjänstens huvudnamn och autentisera sig som tjänstens huvudnamn genom att tillhandahålla ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="abe9c-175">In this section, you perform the steps to create a service principal for an Azure Active Directory application, assign a role to the service principal, and authenticate as the service principal by providing a certificate.</span></span> <span data-ttu-id="abe9c-176">Kör följande kommandon för att skapa ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="abe9c-176">Run the following commands to create an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="abe9c-177">Klistra in följande cmdlets i PowerShell-konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="abe9c-177">Paste the following cmdlets in the PowerShell console window.</span></span> <span data-ttu-id="abe9c-178">Kontrollera att värdet som du anger för den **- DisplayName** egenskapen är unika.</span><span class="sxs-lookup"><span data-stu-id="abe9c-178">Make sure the value you specify for the **-DisplayName** property is unique.</span></span> <span data-ttu-id="abe9c-179">Värden för **- webbsida** och **- IdentiferUris** är platshållarvärdena och kan inte verifieras.</span><span class="sxs-lookup"><span data-stu-id="abe9c-179">Also, the values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

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
2. <span data-ttu-id="abe9c-180">Skapa ett huvudnamn för tjänsten med hjälp av program-ID.</span><span class="sxs-lookup"><span data-stu-id="abe9c-180">Create a service principal using the application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="abe9c-181">Ge service principal åtkomst till Data Lake Store-mapp och fil som du kommer åt från HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="abe9c-181">Grant the service principal access to the Data Lake Store folder and the file that you will access from the HDInsight cluster.</span></span> <span data-ttu-id="abe9c-182">Kodfragmentet nedan ger åtkomst till roten på Data Lake Store-konto (dit du kopierade Exempeldatafilen), och själva filen.</span><span class="sxs-lookup"><span data-stu-id="abe9c-182">The snippet below provides access to the root of the Data Lake Store account (where you copied the sample data file), and the file itself.</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="abe9c-183">Skapa ett kluster i HDInsight Linux med Data Lake Store som ytterligare lagringsutrymme</span><span class="sxs-lookup"><span data-stu-id="abe9c-183">Create an HDInsight Linux cluster with Data Lake Store as additional storage</span></span>

<span data-ttu-id="abe9c-184">I det här avsnittet skapar vi ett HDInsight Hadoop Linux-kluster med Data Lake Store som ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="abe9c-184">In this section, we create an HDInsight Hadoop Linux cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="abe9c-185">HDInsight-klustret och Data Lake Store måste vara på samma plats för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="abe9c-185">For this release, the HDInsight cluster and the Data Lake Store must be in the same location.</span></span>

1. <span data-ttu-id="abe9c-186">Börja med att hämta prenumerationen klient-ID.</span><span class="sxs-lookup"><span data-stu-id="abe9c-186">Start with retrieving the subscription tenant ID.</span></span> <span data-ttu-id="abe9c-187">Du behöver som senare.</span><span class="sxs-lookup"><span data-stu-id="abe9c-187">You will need that later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. <span data-ttu-id="abe9c-188">Den här versionen kan för Hadoop-kluster Data Lake Store endast användas som ett ytterligare lagringsutrymme för klustret.</span><span class="sxs-lookup"><span data-stu-id="abe9c-188">For this release, for a Hadoop cluster, Data Lake Store can only be used as an additional storage for the cluster.</span></span> <span data-ttu-id="abe9c-189">Standard-lagringen kommer fortfarande att Azure storage-blobbar (WASB).</span><span class="sxs-lookup"><span data-stu-id="abe9c-189">The default storage will still be the Azure storage blobs (WASB).</span></span> <span data-ttu-id="abe9c-190">Så måste vi först skapa storage-konto och behållare som krävs för klustret.</span><span class="sxs-lookup"><span data-stu-id="abe9c-190">So, we'll first create the storage account and storage containers required for the cluster.</span></span>

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. <span data-ttu-id="abe9c-191">Skapa HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="abe9c-191">Create the HDInsight cluster.</span></span> <span data-ttu-id="abe9c-192">Använd följande cmdlets.</span><span class="sxs-lookup"><span data-stu-id="abe9c-192">Use the following cmdlets.</span></span>

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    <span data-ttu-id="abe9c-193">Du bör se utdata visar information om kluster efter cmdleten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="abe9c-193">After the cmdlet successfully completes, you should see an output listing the cluster details.</span></span>


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a><span data-ttu-id="abe9c-194">Kör testjobb i HDInsight-klustret för att använda Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-194">Run test jobs on the HDInsight cluster to use the Data Lake Store</span></span>
<span data-ttu-id="abe9c-195">När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på klustret för att testa att HDInsight-klustret har åtkomst till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-195">After you have configured an HDInsight cluster, you can run test jobs on the cluster to test that the HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="abe9c-196">Om du vill göra det, kommer vi kör ett Hive-jobb som skapar en tabell med exempeldata som du tidigare har överförts till din Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-196">To do so, we will run a sample Hive job that creates a table using the sample data that you uploaded earlier to your Data Lake Store.</span></span>

<span data-ttu-id="abe9c-197">I det här avsnittet kommer du att SSH i HDInsight Linux-kluster du skapat och kör den en exempelfråga Hive.</span><span class="sxs-lookup"><span data-stu-id="abe9c-197">In this section you will SSH into the HDInsight Linux cluster you created and run the a sample Hive query.</span></span>

* <span data-ttu-id="abe9c-198">Om du använder en Windows-klient till SSH till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="abe9c-198">If you are using a Windows client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="abe9c-199">Om du använder en Linux-klient till SSH till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="abe9c-199">If you are using a Linux client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

1. <span data-ttu-id="abe9c-200">När du är ansluten, startar du Hive-CLI med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="abe9c-200">Once connected, start the Hive CLI by using the following command:</span></span>

        hive
2. <span data-ttu-id="abe9c-201">Med hjälp av CLI, ange följande instruktioner för att skapa en ny tabell med namnet **fordon** med exempeldata i Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="abe9c-201">Using the CLI, enter the following statements to create a new table named **vehicles** by using the sample data in the Data Lake Store:</span></span>

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    <span data-ttu-id="abe9c-202">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="abe9c-202">You should see an output similar to the following:</span></span>

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

## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="abe9c-203">Åtkomst till Data Lake Store med hjälp av HDFS-kommandon</span><span class="sxs-lookup"><span data-stu-id="abe9c-203">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="abe9c-204">När du har konfigurerat HDInsight-klustret för att använda Data Lake Store kan använda du HDFS-gränssnittskommandon tillgång till store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-204">Once you have configured the HDInsight cluster to use Data Lake Store, you can use the HDFS shell commands to access the store.</span></span>

<span data-ttu-id="abe9c-205">I det här avsnittet kommer du att SSH i HDInsight Linux-kluster du skapat och kör HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="abe9c-205">In this section you will SSH into the HDInsight Linux cluster you created and run the HDFS commands.</span></span>

* <span data-ttu-id="abe9c-206">Om du använder en Windows-klient till SSH till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="abe9c-206">If you are using a Windows client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="abe9c-207">Om du använder en Linux-klient till SSH till klustret finns [använda SSH med Linux-baserade Hadoop i HDInsight från Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="abe9c-207">If you are using a Linux client to SSH into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

<span data-ttu-id="abe9c-208">När du är ansluten, Använd kommandot filesystem HDFS för att visa filer i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-208">Once connected, use the following HDFS filesystem command to list the files in the Data Lake Store.</span></span>

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

<span data-ttu-id="abe9c-209">Du bör nu se filen som du tidigare har överförts till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="abe9c-209">This should list the file that you uploaded earlier to the Data Lake Store.</span></span>

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

<span data-ttu-id="abe9c-210">Du kan också använda den `hdfs dfs -put` kommando för att överföra filer till Data Lake Store och sedan använda `hdfs dfs -ls` att kontrollera om filerna som har laddats upp.</span><span class="sxs-lookup"><span data-stu-id="abe9c-210">You can also use the `hdfs dfs -put` command to upload some files to the Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="abe9c-211">Se även</span><span class="sxs-lookup"><span data-stu-id="abe9c-211">See Also</span></span>
* [<span data-ttu-id="abe9c-212">Portalen: Skapa ett HDInsight-kluster om du vill använda Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="abe9c-212">Portal: Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
