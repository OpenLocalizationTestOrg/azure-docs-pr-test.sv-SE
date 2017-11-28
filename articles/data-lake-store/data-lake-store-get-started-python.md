---
title: "aaaUse hello Python SDK tooget igång med Azure Data Lake Store | Microsoft Docs"
description: "Lär dig hur toouse Python SDK toowork med Data Lake Store-konton och hello-filsystemet."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 7061fdf25ef607608bab618a20ddd3d6fc7af01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-python"></a><span data-ttu-id="31545-103">Komma igång med Azure Data Lake Store med hjälp av Python</span><span class="sxs-lookup"><span data-stu-id="31545-103">Get started with Azure Data Lake Store using Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="31545-104">Portal</span><span class="sxs-lookup"><span data-stu-id="31545-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="31545-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31545-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="31545-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="31545-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="31545-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="31545-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="31545-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="31545-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="31545-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31545-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="31545-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="31545-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="31545-111">Python</span><span class="sxs-lookup"><span data-stu-id="31545-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="31545-112">Lär dig hur toouse hello Python SDK för Azure och Azure Data Lake Store tooperform grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, osv. Mer information om Data Lake finns i [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="31545-112">Learn how toouse hello Python SDK for Azure and Azure Data Lake Store tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31545-113">Krav</span><span class="sxs-lookup"><span data-stu-id="31545-113">Prerequisites</span></span>

* <span data-ttu-id="31545-114">**Python**.</span><span class="sxs-lookup"><span data-stu-id="31545-114">**Python**.</span></span> <span data-ttu-id="31545-115">Du kan hämta Python [här](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="31545-115">You can download Python from [here](https://www.python.org/downloads/).</span></span> <span data-ttu-id="31545-116">I den här artikeln används Python 3.5.2.</span><span class="sxs-lookup"><span data-stu-id="31545-116">This article uses Python 3.5.2.</span></span>

* <span data-ttu-id="31545-117">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="31545-117">**An Azure subscription**.</span></span> <span data-ttu-id="31545-118">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="31545-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="31545-119">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="31545-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="31545-120">Du kan använda hello Azure AD application tooauthenticate hello Data Lake Store-program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="31545-120">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="31545-121">Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**.</span><span class="sxs-lookup"><span data-stu-id="31545-121">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="31545-122">Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="31545-122">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="install-hello-modules"></a><span data-ttu-id="31545-123">Installera hello-moduler</span><span class="sxs-lookup"><span data-stu-id="31545-123">Install hello modules</span></span>

<span data-ttu-id="31545-124">toowork med Data Lake Store med hjälp av Python, behöver du tooinstall tre moduler.</span><span class="sxs-lookup"><span data-stu-id="31545-124">toowork with Data Lake Store using Python, you need tooinstall three modules.</span></span>

* <span data-ttu-id="31545-125">Hej `azure-mgmt-resource` modul.</span><span class="sxs-lookup"><span data-stu-id="31545-125">hello `azure-mgmt-resource` module.</span></span> <span data-ttu-id="31545-126">Den här modulen innehåller Azure-moduler för Active Directory osv.</span><span class="sxs-lookup"><span data-stu-id="31545-126">This includes Azure modules for Active Directory, etc..</span></span>
* <span data-ttu-id="31545-127">Hej `azure-mgmt-datalake-store` modul.</span><span class="sxs-lookup"><span data-stu-id="31545-127">hello `azure-mgmt-datalake-store` module.</span></span> <span data-ttu-id="31545-128">Detta inkluderar hanteringsåtgärder för hello Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="31545-128">This includes hello Azure Data Lake Store account management operations.</span></span> <span data-ttu-id="31545-129">Mer information om den här modulen finns i [referensen för Azure Data Lake Store Management-modulen](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span><span class="sxs-lookup"><span data-stu-id="31545-129">For more information on this module, see [Azure Data Lake Store Management module reference](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).</span></span>
* <span data-ttu-id="31545-130">Hej `azure-datalake-store` modul.</span><span class="sxs-lookup"><span data-stu-id="31545-130">hello `azure-datalake-store` module.</span></span> <span data-ttu-id="31545-131">Detta inkluderar hello Azure Data Lake Store filsystemsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="31545-131">This includes hello Azure Data Lake Store filesystem operations.</span></span> <span data-ttu-id="31545-132">Mer information om den här modulen finns i [referensen för Azure Data Lake Store Filesystem-modulen](http://azure-datalake-store.readthedocs.io/en/latest/).</span><span class="sxs-lookup"><span data-stu-id="31545-132">For more information on this module, see [Azure Data Lake Store Filesystem module reference](http://azure-datalake-store.readthedocs.io/en/latest/).</span></span>

<span data-ttu-id="31545-133">Använd följande kommandon tooinstall hello moduler hello.</span><span class="sxs-lookup"><span data-stu-id="31545-133">Use hello following commands tooinstall hello modules.</span></span>

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a><span data-ttu-id="31545-134">Skapa ett nytt Python-program</span><span class="sxs-lookup"><span data-stu-id="31545-134">Create a new Python application</span></span>

1. <span data-ttu-id="31545-135">I hello IDE du väljer att skapa ett nytt Python-program, till exempel **mysample.py**.</span><span class="sxs-lookup"><span data-stu-id="31545-135">In hello IDE of your choice create a new Python application, for example, **mysample.py**.</span></span>

2. <span data-ttu-id="31545-136">Lägg till följande rader tooimport hello krävs moduler hello</span><span class="sxs-lookup"><span data-stu-id="31545-136">Add hello following lines tooimport hello required modules</span></span>

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. <span data-ttu-id="31545-137">Spara ändringarna toomysample.py.</span><span class="sxs-lookup"><span data-stu-id="31545-137">Save changes toomysample.py.</span></span>

## <a name="authentication"></a><span data-ttu-id="31545-138">Autentisering</span><span class="sxs-lookup"><span data-stu-id="31545-138">Authentication</span></span>

<span data-ttu-id="31545-139">I det här avsnittet pratar vi om hello olika sätt tooauthenticate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="31545-139">In this section, we talk about hello different ways tooauthenticate with Azure AD.</span></span> <span data-ttu-id="31545-140">hello tillgängliga alternativ är:</span><span class="sxs-lookup"><span data-stu-id="31545-140">hello options available are:</span></span>

* <span data-ttu-id="31545-141">Slutanvändarautentisering</span><span class="sxs-lookup"><span data-stu-id="31545-141">End-user authentication</span></span>
* <span data-ttu-id="31545-142">Tjänst-till-tjänst-autentisering</span><span class="sxs-lookup"><span data-stu-id="31545-142">Service-to-service authentication</span></span>
* <span data-ttu-id="31545-143">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="31545-143">Multi-factor authentication</span></span>

<span data-ttu-id="31545-144">Du måste använda dessa autentiseringsalternativ för både kontohanterings- och filsystemhanteringsmoduler.</span><span class="sxs-lookup"><span data-stu-id="31545-144">You must use these authentication options for both account management and filesystem management modules.</span></span>

### <a name="end-user-authentication-for-account-management"></a><span data-ttu-id="31545-145">Slutanvändarautentisering för kontohantering</span><span class="sxs-lookup"><span data-stu-id="31545-145">End-user authentication for account management</span></span>

<span data-ttu-id="31545-146">Använd den här tooauthenticate med Azure AD för kontohanteringsåtgärder (skapa/ta bort Data Lake Store-konto, osv.).</span><span class="sxs-lookup"><span data-stu-id="31545-146">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="31545-147">Du måste ange användarnamn och lösenord för en Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="31545-147">You must provide username and password for an Azure AD user.</span></span> <span data-ttu-id="31545-148">Observera hello användaren inte ska konfigureras för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="31545-148">Note that hello user should not be configured for multi-factor authentication.</span></span>

    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a><span data-ttu-id="31545-149">Slutanvändarautentisering för filsystemsåtgärder</span><span class="sxs-lookup"><span data-stu-id="31545-149">End-user authentication for filesystem operations</span></span>

<span data-ttu-id="31545-150">Använd den här tooauthenticate med Azure AD för filsystemsåtgärder (skapa mappen, uppladdningsfilen osv.).</span><span class="sxs-lookup"><span data-stu-id="31545-150">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="31545-151">Använd den här metoden med ett befintligt **inbyggt Azure AD-klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="31545-151">Use this with an existing Azure AD **native client** application.</span></span> <span data-ttu-id="31545-152">hello Azure AD-användare som du ange autentiseringsuppgifter för ska inte konfigureras för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="31545-152">hello Azure AD user you provide credentials for should not be configured for multi-factor authentication.</span></span>

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a><span data-ttu-id="31545-153">”Tjänst till tjänst”-autentisering med klienthemlighet för kontohantering</span><span class="sxs-lookup"><span data-stu-id="31545-153">Service-to-service authentication with client secret for account management</span></span>

<span data-ttu-id="31545-154">Använd den här tooauthenticate med Azure AD för kontohanteringsåtgärder (skapa/ta bort Data Lake Store-konto, osv.).</span><span class="sxs-lookup"><span data-stu-id="31545-154">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="31545-155">Hej följande fragment kan vara används tooauthenticate tillämpningsprogrammet icke-interaktivt, använder hello klienthemlighet för ett program / service principal.</span><span class="sxs-lookup"><span data-stu-id="31545-155">hello following snippet can be used tooauthenticate your application non-interactively, using hello client secret for an application / service principal.</span></span> <span data-ttu-id="31545-156">Använd detta med ett befintligt Azure AD-webbappsprogram.</span><span class="sxs-lookup"><span data-stu-id="31545-156">Use this with an existing Azure AD "Web App" application.</span></span>

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a><span data-ttu-id="31545-157">Tjänst-till-tjänst-autentisering med klienthemlighet för filsystemsåtgärder</span><span class="sxs-lookup"><span data-stu-id="31545-157">Service-to-service authentication with client secret for filesystem operations</span></span>

<span data-ttu-id="31545-158">Använd den här tooauthenticate med Azure AD för filsystemsåtgärder (skapa mappen, uppladdningsfilen osv.).</span><span class="sxs-lookup"><span data-stu-id="31545-158">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="31545-159">Hej följande fragment kan vara används tooauthenticate tillämpningsprogrammet icke-interaktivt, använder hello klienthemlighet för ett program / service principal.</span><span class="sxs-lookup"><span data-stu-id="31545-159">hello following snippet can be used tooauthenticate your application non-interactively, using hello client secret for an application / service principal.</span></span> <span data-ttu-id="31545-160">Använd detta med ett befintligt Azure AD-webbappsprogram.</span><span class="sxs-lookup"><span data-stu-id="31545-160">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a><span data-ttu-id="31545-161">Multifaktorautentisering för kontohantering</span><span class="sxs-lookup"><span data-stu-id="31545-161">Multi-factor authentication for account management</span></span>

<span data-ttu-id="31545-162">Använd den här tooauthenticate med Azure AD för kontohanteringsåtgärder (skapa/ta bort Data Lake Store-konto, osv.).</span><span class="sxs-lookup"><span data-stu-id="31545-162">Use this tooauthenticate with Azure AD for account management operations (create/delete Data Lake Store account, etc.).</span></span> <span data-ttu-id="31545-163">hello följande kodavsnitt kan vara används tooauthenticate ditt program med hjälp av multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="31545-163">hello following snippet can be used tooauthenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="31545-164">Använd detta med ett befintligt Azure AD-webbappsprogram.</span><span class="sxs-lookup"><span data-stu-id="31545-164">Use this with an existing Azure AD "Web App" application.</span></span>

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    credentials = AADTokenCredentials(mgmt_token, client_id)

### <a name="multi-factor-authentication-for-filesystem-management"></a><span data-ttu-id="31545-165">Multifaktorautentisering för hantering av filsystem</span><span class="sxs-lookup"><span data-stu-id="31545-165">Multi-factor authentication for filesystem management</span></span>

<span data-ttu-id="31545-166">Använd den här tooauthenticate med Azure AD för filsystemsåtgärder (skapa mappen, uppladdningsfilen osv.).</span><span class="sxs-lookup"><span data-stu-id="31545-166">Use this tooauthenticate with Azure AD for filesystem operations (create folder, upload file, etc.).</span></span> <span data-ttu-id="31545-167">hello följande kodavsnitt kan vara används tooauthenticate ditt program med hjälp av multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="31545-167">hello following snippet can be used tooauthenticate your application using multi-factor authentication.</span></span> <span data-ttu-id="31545-168">Använd detta med ett befintligt Azure AD-webbappsprogram.</span><span class="sxs-lookup"><span data-stu-id="31545-168">Use this with an existing Azure AD "Web App" application.</span></span>

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="31545-169">Skapa en Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="31545-169">Create an Azure Resource Group</span></span>

<span data-ttu-id="31545-170">Använd följande kodfragment toocreate Azure-resursgrupp hello:</span><span class="sxs-lookup"><span data-stu-id="31545-170">Use hello following code snippet toocreate an Azure Resource Group:</span></span>

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

## <a name="create-clients-and-data-lake-store-account"></a><span data-ttu-id="31545-171">Skapa klienter och Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="31545-171">Create clients and Data Lake Store account</span></span>

<span data-ttu-id="31545-172">hello följande fragment först skapar hello Data Lake Store-konto klienten.</span><span class="sxs-lookup"><span data-stu-id="31545-172">hello following snippet first creates hello Data Lake Store account client.</span></span> <span data-ttu-id="31545-173">Den använder hello klienten objektet toocreate ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="31545-173">It uses hello client object toocreate a Data Lake Store account.</span></span> <span data-ttu-id="31545-174">Slutligen skapar hello kodutdrag ett objekt för filsystem.</span><span class="sxs-lookup"><span data-stu-id="31545-174">Finally, hello snippet creates a filesystem client object.</span></span>

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

## <a name="list-hello-data-lake-store-accounts"></a><span data-ttu-id="31545-175">Lista över hello Data Lake Store-konton</span><span class="sxs-lookup"><span data-stu-id="31545-175">List hello Data Lake Store accounts</span></span>

    ## List hello existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="create-a-directory"></a><span data-ttu-id="31545-176">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="31545-176">Create a directory</span></span>

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a><span data-ttu-id="31545-177">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="31545-177">Upload a file</span></span>


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a><span data-ttu-id="31545-178">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="31545-178">Download a file</span></span>

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a><span data-ttu-id="31545-179">Ta bort en katalog</span><span class="sxs-lookup"><span data-stu-id="31545-179">Delete a directory</span></span>

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="see-also"></a><span data-ttu-id="31545-180">Se även</span><span class="sxs-lookup"><span data-stu-id="31545-180">See also</span></span>

- [<span data-ttu-id="31545-181">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="31545-181">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
- [<span data-ttu-id="31545-182">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="31545-182">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [<span data-ttu-id="31545-183">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="31545-183">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
- [<span data-ttu-id="31545-184">Data Lake Store .NET SDK-referens</span><span class="sxs-lookup"><span data-stu-id="31545-184">Data Lake Store .NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt581387.aspx)
- [<span data-ttu-id="31545-185">Data Lake Store REST-referens</span><span class="sxs-lookup"><span data-stu-id="31545-185">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
