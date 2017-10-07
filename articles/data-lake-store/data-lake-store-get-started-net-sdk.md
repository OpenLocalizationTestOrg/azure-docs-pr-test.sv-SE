---
title: aaaUse hello .NET SDK toodevelop program i Azure Data Lake Store | Microsoft Docs
description: "Använda Azure Data Lake Store .NET SDK toocreate ett Data Lake Store-konto och utföra grundläggande åtgärder i hello Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: nitinme
ms.openlocfilehash: cb3a1dfb2f6379f728069d66b0ee77ce0f838fe7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a><span data-ttu-id="ad2ac-103">Kom igång med Azure Data Lake Store med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ad2ac-103">Get started with Azure Data Lake Store using .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad2ac-104">Portal</span><span class="sxs-lookup"><span data-stu-id="ad2ac-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="ad2ac-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad2ac-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="ad2ac-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="ad2ac-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="ad2ac-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="ad2ac-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="ad2ac-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="ad2ac-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="ad2ac-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ad2ac-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="ad2ac-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="ad2ac-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="ad2ac-111">Python</span><span class="sxs-lookup"><span data-stu-id="ad2ac-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="ad2ac-112">Lär dig hur toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, osv. Mer information om Data Lake finns i [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad2ac-112">Learn how toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad2ac-113">Krav</span><span class="sxs-lookup"><span data-stu-id="ad2ac-113">Prerequisites</span></span>
* <span data-ttu-id="ad2ac-114">**Visual Studio 2013, 2015 eller 2017**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-114">**Visual Studio 2013, 2015, or 2017**.</span></span> <span data-ttu-id="ad2ac-115">hello instruktionerna nedan använder Visual Studio 2015 Update 2.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-115">hello instructions below use Visual Studio 2015 Update 2.</span></span>

* <span data-ttu-id="ad2ac-116">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-116">**An Azure subscription**.</span></span> <span data-ttu-id="ad2ac-117">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad2ac-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="ad2ac-118">**Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-118">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="ad2ac-119">Anvisningar för hur toocreate ett konto, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ad2ac-119">For instructions on how toocreate an account, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

* <span data-ttu-id="ad2ac-120">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-120">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="ad2ac-121">Du kan använda hello Azure AD application tooauthenticate hello Data Lake Store-program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-121">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="ad2ac-122">Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-122">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="ad2ac-123">Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="ad2ac-123">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-a-net-application"></a><span data-ttu-id="ad2ac-124">Skapa ett .NET-program</span><span class="sxs-lookup"><span data-stu-id="ad2ac-124">Create a .NET application</span></span>
1. <span data-ttu-id="ad2ac-125">Öppna Visual Studio och skapa ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-125">Open Visual Studio and create a console application.</span></span>
2. <span data-ttu-id="ad2ac-126">Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-126">From hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="ad2ac-127">Från **nytt projekt**anger eller väljer hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="ad2ac-127">From **New Project**, type or select hello following values:</span></span>

   | <span data-ttu-id="ad2ac-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ad2ac-128">Property</span></span> | <span data-ttu-id="ad2ac-129">Värde</span><span class="sxs-lookup"><span data-stu-id="ad2ac-129">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="ad2ac-130">Kategori</span><span class="sxs-lookup"><span data-stu-id="ad2ac-130">Category</span></span> |<span data-ttu-id="ad2ac-131">Mallar/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="ad2ac-131">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="ad2ac-132">Mall</span><span class="sxs-lookup"><span data-stu-id="ad2ac-132">Template</span></span> |<span data-ttu-id="ad2ac-133">Konsolprogram</span><span class="sxs-lookup"><span data-stu-id="ad2ac-133">Console Application</span></span> |
   | <span data-ttu-id="ad2ac-134">Namn</span><span class="sxs-lookup"><span data-stu-id="ad2ac-134">Name</span></span> |<span data-ttu-id="ad2ac-135">CreateADLApplication</span><span class="sxs-lookup"><span data-stu-id="ad2ac-135">CreateADLApplication</span></span> |
4. <span data-ttu-id="ad2ac-136">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-136">Click **OK** toocreate hello project.</span></span>
5. <span data-ttu-id="ad2ac-137">Lägg till hello Nuget paket tooyour projektet.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-137">Add hello Nuget packages tooyour project.</span></span>

   1. <span data-ttu-id="ad2ac-138">Högerklicka på hello projektnamnet i hello Solution Explorer och klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-138">Right-click hello project name in hello Solution Explorer and click **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ad2ac-139">I hello **Nuget Package Manager** kontrollerar du att som **paketkällan** har angetts för**nuget.org** och att **inkludera förhandsversion** kryssruta har valts.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-139">In hello **Nuget Package Manager** tab, make sure that **Package source** is set too**nuget.org** and that **Include prerelease** check box is selected.</span></span>
   3. <span data-ttu-id="ad2ac-140">Söka efter och installera hello följande NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="ad2ac-140">Search for and install hello following NuGet packages:</span></span>

      * <span data-ttu-id="ad2ac-141">`Microsoft.Azure.Management.DataLake.Store` – I den här självstudiekursen används v2.1.3-förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-141">`Microsoft.Azure.Management.DataLake.Store` - This tutorial uses v2.1.3-preview.</span></span>
      * <span data-ttu-id="ad2ac-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` –I den här självstudiekursen används v2.2.12.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-142">`Microsoft.Rest.ClientRuntime.Azure.Authentication` - This tutorial uses v2.2.12.</span></span>

        <span data-ttu-id="ad2ac-143">![Lägg till en Nuget-källa](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Skapa ett nytt Azure Data Lake-konto")</span><span class="sxs-lookup"><span data-stu-id="ad2ac-143">![Add a Nuget source](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Create a new Azure Data Lake account")</span></span>
   4. <span data-ttu-id="ad2ac-144">Stäng hello **Nuget Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-144">Close hello **Nuget Package Manager**.</span></span>
6. <span data-ttu-id="ad2ac-145">Öppna **Program.cs**, ta bort hello befintlig kod och inkludera hello följande instruktioner tooadd referenser toonamespaces.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-145">Open **Program.cs**, delete hello existing code, and then include hello following statements tooadd references toonamespaces.</span></span>

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. <span data-ttu-id="ad2ac-146">Deklarera hello variabler som visas nedan och ange hello värden för Data Lake Store-namn och hello resursgruppens namn som redan finns.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-146">Declare hello variables as shown below, and provide hello values for Data Lake Store name and hello resource group name that already exist.</span></span> <span data-ttu-id="ad2ac-147">Kontrollera också hello lokala sökvägen och filnamnet du anger här måste finnas på hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-147">Also, make sure hello local path and file name you provide here must exist on hello computer.</span></span> <span data-ttu-id="ad2ac-148">Lägg till följande kodfragment efter hello namnrymdsdeklarationer hello.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-148">Add hello following code snippet after hello namespace declarations.</span></span>

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with hello name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with hello name of hello resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

<span data-ttu-id="ad2ac-149">I hello återstående avsnitt i hello artikel, ser du hur toouse hello tillgängliga .NET metoder tooperform åtgärder, till exempel autentisering, ladda upp filen osv.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-149">In hello remaining sections of hello article, you can see how toouse hello available .NET methods tooperform operations such as authentication, file upload, etc.</span></span>

## <a name="authentication"></a><span data-ttu-id="ad2ac-150">Autentisering</span><span class="sxs-lookup"><span data-stu-id="ad2ac-150">Authentication</span></span>

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a><span data-ttu-id="ad2ac-151">Om du använder autentisering för slutanvändare (rekommenderas för den här kursen)</span><span class="sxs-lookup"><span data-stu-id="ad2ac-151">If you are using end-user authentication (recommended for this tutorial)</span></span>

<span data-ttu-id="ad2ac-152">Använda detta med en befintlig Azure AD programspecifika tooauthenticate programmet **interaktivt**, vilket betyder att du kommer att uppmanas tooenter dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-152">Use this with an existing Azure AD native application tooauthenticate your application **interactively**, which means you will be prompted tooenter your Azure credentials.</span></span>

<span data-ttu-id="ad2ac-153">Lätt att använda använder hello kodfragmentet nedan standardvärden för klient-ID och omdirigerings-URI som fungerar med alla Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-153">For ease of use, hello snippet below uses default values for client ID and redirect URI that will work with any Azure subscription.</span></span> <span data-ttu-id="ad2ac-154">toohelp du den här kursen snabbare, rekommenderar vi du använder den här metoden.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-154">toohelp you complete this tutorial faster, we recommend you use this approach.</span></span> <span data-ttu-id="ad2ac-155">Hello kodfragmentet nedan, anger du bara hello värdet för klient-ID.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-155">In hello snippet below, just provide hello value for your tenant ID.</span></span> <span data-ttu-id="ad2ac-156">Du kan hämta den med hjälp av instruktionerna på hello [skapa ett Active Directory-program](data-lake-store-end-user-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="ad2ac-156">You can retrieve it using hello instructions provided at [Create an Active Directory Application](data-lake-store-end-user-authenticate-using-active-directory.md).</span></span>

    // User login via interactive popup
    // Use hello client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with hello user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

<span data-ttu-id="ad2ac-157">Ett par saker tooknow om den här fragment ovan:</span><span class="sxs-lookup"><span data-stu-id="ad2ac-157">A couple of things tooknow about this snippet above:</span></span>

* <span data-ttu-id="ad2ac-158">toohelp du kursen hello snabbare, den här fragment använder en en Azure AD domain och klient-ID som är tillgängligt som standard för alla Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-158">toohelp you complete hello tutorial faster, this snippet uses an an Azure AD domain and client ID that is available by default for all Azure subscriptions.</span></span> <span data-ttu-id="ad2ac-159">Så kan du **använder detta utdrag i befintligt skick i ditt program**.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-159">So, you can **use this snippet as-is in your application**.</span></span>
* <span data-ttu-id="ad2ac-160">Men om du vill toouse din egen Azure AD-domän och klient-ID för programmet måste du skapa ett enhetligt Azure AD-program och sedan använda hello Azure AD klient-ID, klient-ID och omdirigerings-URI för programmet hello skapat du.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-160">However, if you do want toouse your own Azure AD domain and application client ID, you must create an Azure AD native application and then use hello Azure AD tenant ID, client ID, and redirect URI for hello application you created.</span></span> <span data-ttu-id="ad2ac-161">Se [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) (Skapa ett Active Directory-program för slutanvändarautentisering med Data Lake Store) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-161">See [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) for instructions.</span></span>

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a><span data-ttu-id="ad2ac-162">Om du använder serviceautentisering med klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="ad2ac-162">If you are using service-to-service authentication with client secret</span></span>
<span data-ttu-id="ad2ac-163">Hej följande fragment kan vara används tooauthenticate programmet **icke-interaktivt**, med hjälp av hello klienthemlighet / key för ett program / service principal.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-163">hello following snippet can be used tooauthenticate your application **non-interactively**, using hello client secret / key for an application / service principal.</span></span> <span data-ttu-id="ad2ac-164">Använd det här med ett befintligt Azure AD-program av typen webbapp.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-164">Use this with an existing Azure AD "Web App" Application.</span></span> <span data-ttu-id="ad2ac-165">Anvisningar för hur toocreate hello Azure AD-webbprogram och hur tooretrieve hello klient-ID och klienthemlighet som krävs i hello kodfragmentet nedan, se [skapa ett Active Directory-program för tjänst-till-tjänst-autentisering med Data Datasjölager](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="ad2ac-165">For instructions on how toocreate hello Azure AD web application and how tooretrieve hello client ID and client secret required in hello snippet below, see [Create an Active Directory Application for service-to-service authentication with Data Lake Store](data-lake-store-authenticate-using-active-directory.md).</span></span>

    // Service principal / appplication authentication with client secret / key
    // Use hello client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a><span data-ttu-id="ad2ac-166">Om du använder serviceautentisering med certifikat</span><span class="sxs-lookup"><span data-stu-id="ad2ac-166">If you are using service-to-service authentication with certificate</span></span>

<span data-ttu-id="ad2ac-167">Som ett tredje alternativ, hello följande fragment kan vara används tooauthenticate programmet **icke-interaktivt**, använder hello certifikat för ett Azure Active Directory-program / service principal.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-167">As a third option, hello following snippet can be used tooauthenticate your application **non-interactively**, using hello certificate for an Azure Active Directory application / service principal.</span></span> <span data-ttu-id="ad2ac-168">Använd det här med ett befintligt [Azure AD-program med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="ad2ac-168">Use this with an existing [Azure AD Application with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

    // Service principal / application authentication with certificate
    // Use hello client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a><span data-ttu-id="ad2ac-169">Skapa klientobjekt</span><span class="sxs-lookup"><span data-stu-id="ad2ac-169">Create client objects</span></span>
<span data-ttu-id="ad2ac-170">hello följande kodavsnitt skapar hello Data Lake Store-konto och filsystem klientobjekt som används för tooissue begär toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-170">hello following snippet creates hello Data Lake Store account and filesystem client objects, which are used tooissue requests toohello service.</span></span>

    // Create client objects and set hello subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a><span data-ttu-id="ad2ac-171">Lista över alla Data Lake Store-konton inom en prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad2ac-171">List all Data Lake Store accounts within a subscription</span></span>
<span data-ttu-id="ad2ac-172">hello visar följande utdrag alla Data Lake Store-konton inom en viss Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-172">hello following snippet lists all Data Lake Store accounts within a given Azure subscription.</span></span>

    // List all ADLS accounts within hello subscription
    public static async Task<List<DataLakeStoreAccount>> ListAdlStoreAccounts()
    {
        var response = await _adlsClient.Account.ListAsync();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a><span data-ttu-id="ad2ac-173">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="ad2ac-173">Create a directory</span></span>
<span data-ttu-id="ad2ac-174">hello följande fragment visas en `CreateDirectory` metod som du kan använda toocreate en katalog i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-174">hello following snippet shows a `CreateDirectory` method that you can use toocreate a directory within a Data Lake Store account.</span></span>

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a><span data-ttu-id="ad2ac-175">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="ad2ac-175">Upload a file</span></span>
<span data-ttu-id="ad2ac-176">hello följande fragment visas en `UploadFile` metod som du kan använda tooupload filer tooa Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-176">hello following snippet shows an `UploadFile` method that you can use tooupload files tooa Data Lake Store account.</span></span>

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

<span data-ttu-id="ad2ac-177">hello SDK stöder rekursiv överföring och hämtning mellan en lokal sökväg och en sökväg till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-177">hello SDK supports recursive upload and download between a local file path and a Data Lake Store file path.</span></span>    

## <a name="get-file-or-directory-info"></a><span data-ttu-id="ad2ac-178">Hämta fil- eller kataloginformation</span><span class="sxs-lookup"><span data-stu-id="ad2ac-178">Get file or directory info</span></span>
<span data-ttu-id="ad2ac-179">hello följande fragment visas en `GetItemInfo` metod som du kan använda tooretrieve information om en fil eller katalog som är tillgängliga i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-179">hello following snippet shows a `GetItemInfo` method that you can use tooretrieve information about a file or directory available in Data Lake Store.</span></span>

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a><span data-ttu-id="ad2ac-180">Lista över fil eller kataloger</span><span class="sxs-lookup"><span data-stu-id="ad2ac-180">List file or directories</span></span>
<span data-ttu-id="ad2ac-181">hello följande fragment visas en `ListItem` metod som kan använda toolist hello filer och kataloger i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-181">hello following snippet shows a `ListItem` method that can use toolist hello file and directories in a Data Lake Store account.</span></span>

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a><span data-ttu-id="ad2ac-182">Sammanfoga filer</span><span class="sxs-lookup"><span data-stu-id="ad2ac-182">Concatenate files</span></span>
<span data-ttu-id="ad2ac-183">hello följande fragment visas en `ConcatenateFiles` metod du använder tooconcatenate filer.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-183">hello following snippet shows a `ConcatenateFiles` method that you use tooconcatenate files.</span></span>

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-tooa-file"></a><span data-ttu-id="ad2ac-184">Lägg till tooa fil</span><span class="sxs-lookup"><span data-stu-id="ad2ac-184">Append tooa file</span></span>
<span data-ttu-id="ad2ac-185">hello följande fragment visas en `AppendToFile` metod du använder append tooa datafilen lagrad i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-185">hello following snippet shows a `AppendToFile` method that you use append data tooa file already stored in a Data Lake Store account.</span></span>

    // Append toofile
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a><span data-ttu-id="ad2ac-186">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="ad2ac-186">Download a file</span></span>
<span data-ttu-id="ad2ac-187">hello följande fragment visas en `DownloadFile` metod du använder toodownload en fil från ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ad2ac-187">hello following snippet shows a `DownloadFile` method that you use toodownload a file from a Data Lake Store account.</span></span>

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a><span data-ttu-id="ad2ac-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad2ac-188">Next steps</span></span>
* [<span data-ttu-id="ad2ac-189">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ad2ac-189">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="ad2ac-190">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ad2ac-190">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="ad2ac-191">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ad2ac-191">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="ad2ac-192">Data Lake Store .NET SDK-referens</span><span class="sxs-lookup"><span data-stu-id="ad2ac-192">Data Lake Store .NET SDK Reference</span></span>](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [<span data-ttu-id="ad2ac-193">Data Lake Store REST-referens</span><span class="sxs-lookup"><span data-stu-id="ad2ac-193">Data Lake Store REST Reference</span></span>](https://msdn.microsoft.com/library/mt693424.aspx)
