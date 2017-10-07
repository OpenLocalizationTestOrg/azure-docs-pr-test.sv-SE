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
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Kom igång med Azure Data Lake Store med hjälp av .NET SDK
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Lär dig hur toouse hello [Azure Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) tooperform grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, osv. Mer information om Data Lake finns i [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Krav
* **Visual Studio 2013, 2015 eller 2017**. hello instruktionerna nedan använder Visual Studio 2015 Update 2.

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store-konto**. Anvisningar för hur toocreate ett konto, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)

* **Skapa ett program i Azure Active Directory**. Du kan använda hello Azure AD application tooauthenticate hello Data Lake Store-program med Azure AD. Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**. Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Skapa ett .NET-program
1. Öppna Visual Studio och skapa ett konsolprogram.
2. Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.
3. Från **nytt projekt**anger eller väljer hello följande värden:

   | Egenskap | Värde |
   | --- | --- |
   | Kategori |Mallar/Visual C#/Windows |
   | Mall |Konsolprogram |
   | Namn |CreateADLApplication |
4. Klicka på **OK** toocreate hello projektet.
5. Lägg till hello Nuget paket tooyour projektet.

   1. Högerklicka på hello projektnamnet i hello Solution Explorer och klicka på **hantera NuGet-paket**.
   2. I hello **Nuget Package Manager** kontrollerar du att som **paketkällan** har angetts för**nuget.org** och att **inkludera förhandsversion** kryssruta har valts.
   3. Söka efter och installera hello följande NuGet-paket:

      * `Microsoft.Azure.Management.DataLake.Store` – I den här självstudiekursen används v2.1.3-förhandsversionen.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` –I den här självstudiekursen används v2.2.12.

        ![Lägg till en Nuget-källa](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Skapa ett nytt Azure Data Lake-konto")
   4. Stäng hello **Nuget Package Manager**.
6. Öppna **Program.cs**, ta bort hello befintlig kod och inkludera hello följande instruktioner tooadd referenser toonamespaces.

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. Deklarera hello variabler som visas nedan och ange hello värden för Data Lake Store-namn och hello resursgruppens namn som redan finns. Kontrollera också hello lokala sökvägen och filnamnet du anger här måste finnas på hello-datorn. Lägg till följande kodfragment efter hello namnrymdsdeklarationer hello.

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

I hello återstående avsnitt i hello artikel, ser du hur toouse hello tillgängliga .NET metoder tooperform åtgärder, till exempel autentisering, ladda upp filen osv.

## <a name="authentication"></a>Autentisering

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Om du använder autentisering för slutanvändare (rekommenderas för den här kursen)

Använda detta med en befintlig Azure AD programspecifika tooauthenticate programmet **interaktivt**, vilket betyder att du kommer att uppmanas tooenter dina autentiseringsuppgifter för Azure.

Lätt att använda använder hello kodfragmentet nedan standardvärden för klient-ID och omdirigerings-URI som fungerar med alla Azure-prenumerationer. toohelp du den här kursen snabbare, rekommenderar vi du använder den här metoden. Hello kodfragmentet nedan, anger du bara hello värdet för klient-ID. Du kan hämta den med hjälp av instruktionerna på hello [skapa ett Active Directory-program](data-lake-store-end-user-authenticate-using-active-directory.md).

    // User login via interactive popup
    // Use hello client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with hello user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Ett par saker tooknow om den här fragment ovan:

* toohelp du kursen hello snabbare, den här fragment använder en en Azure AD domain och klient-ID som är tillgängligt som standard för alla Azure-prenumerationer. Så kan du **använder detta utdrag i befintligt skick i ditt program**.
* Men om du vill toouse din egen Azure AD-domän och klient-ID för programmet måste du skapa ett enhetligt Azure AD-program och sedan använda hello Azure AD klient-ID, klient-ID och omdirigerings-URI för programmet hello skapat du. Se [Create an Active Directory Application for end-user authentication with Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) (Skapa ett Active Directory-program för slutanvändarautentisering med Data Lake Store) för anvisningar.

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Om du använder serviceautentisering med klienthemlighet
Hej följande fragment kan vara används tooauthenticate programmet **icke-interaktivt**, med hjälp av hello klienthemlighet / key för ett program / service principal. Använd det här med ett befintligt Azure AD-program av typen webbapp. Anvisningar för hur toocreate hello Azure AD-webbprogram och hur tooretrieve hello klient-ID och klienthemlighet som krävs i hello kodfragmentet nedan, se [skapa ett Active Directory-program för tjänst-till-tjänst-autentisering med Data Datasjölager](data-lake-store-authenticate-using-active-directory.md).

    // Service principal / appplication authentication with client secret / key
    // Use hello client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = await ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential);

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Om du använder serviceautentisering med certifikat

Som ett tredje alternativ, hello följande fragment kan vara används tooauthenticate programmet **icke-interaktivt**, använder hello certifikat för ett Azure Active Directory-program / service principal. Använd det här med ett befintligt [Azure AD-program med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    // Service principal / application authentication with certificate
    // Use hello client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());

    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = await ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate);

## <a name="create-client-objects"></a>Skapa klientobjekt
hello följande kodavsnitt skapar hello Data Lake Store-konto och filsystem klientobjekt som används för tooissue begär toohello tjänst.

    // Create client objects and set hello subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds) { SubscriptionId = _subId };
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Lista över alla Data Lake Store-konton inom en prenumeration
hello visar följande utdrag alla Data Lake Store-konton inom en viss Azure-prenumeration.

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

## <a name="create-a-directory"></a>Skapa en katalog
hello följande fragment visas en `CreateDirectory` metod som du kan använda toocreate en katalog i ett Data Lake Store-konto.

    // Create a directory
    public static async Task CreateDirectory(string path)
    {
        await _adlsFileSystemClient.FileSystem.MkdirsAsync(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Överför en fil
hello följande fragment visas en `UploadFile` metod som du kan använda tooupload filer tooa Data Lake Store-konto.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

hello SDK stöder rekursiv överföring och hämtning mellan en lokal sökväg och en sökväg till Data Lake Store.    

## <a name="get-file-or-directory-info"></a>Hämta fil- eller kataloginformation
hello följande fragment visas en `GetItemInfo` metod som du kan använda tooretrieve information om en fil eller katalog som är tillgängliga i Data Lake Store.

    // Get file or directory info
    public static async Task<FileStatusProperties> GetItemInfo(string path)
    {
        return await _adlsFileSystemClient.FileSystem.GetFileStatusAsync(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Lista över fil eller kataloger
hello följande fragment visas en `ListItem` metod som kan använda toolist hello filer och kataloger i ett Data Lake Store-konto.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Sammanfoga filer
hello följande fragment visas en `ConcatenateFiles` metod du använder tooconcatenate filer.

    // Concatenate files
    public static Task ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        await _adlsFileSystemClient.FileSystem.ConcatAsync(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-tooa-file"></a>Lägg till tooa fil
hello följande fragment visas en `AppendToFile` metod du använder append tooa datafilen lagrad i ett Data Lake Store-konto.

    // Append toofile
    public static async Task AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            await _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

## <a name="download-a-file"></a>Hämta en fil
hello följande fragment visas en `DownloadFile` metod du använder toodownload en fil från ett Data Lake Store-konto.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
         _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath);
    }

## <a name="next-steps"></a>Nästa steg
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Data Lake Store .NET SDK-referens](https://docs.microsoft.com/dotnet/api/?view=azuremgmtdatalakestore-2.1.0-preview&term=DataLake.Store)
* [Data Lake Store REST-referens](https://msdn.microsoft.com/library/mt693424.aspx)
